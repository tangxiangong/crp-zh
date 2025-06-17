# Constructors

在C++中，构造函数用于初始化对象。当构造函数执行时，对象的内存空间已经分配完毕，构造函数仅负责执行初始化操作。

Rust并不像C++那样拥有构造函数。在Rust中，创建对象的唯一基本方式是立即初始化其所有成员。Rust中的"构造函数"或"构造方法"这一术语更类似于工厂模式：它是一个与类型关联的静态方法（即没有`self`参数的方法， 译者注：在 Rust 中称为关联函数），该方法会返回对应类型的值/对象。

<div class="comparison">

```cpp
$#include <thread>
$unsigned int cpu_count() {
$    return std::thread::hardware_concurrency();
$}
$
class ThreadPool {
  unsigned int num_threads;

public:
  ThreadPool() : num_threads(cpu_count()) {}
  ThreadPool(unsigned int nt) : num_threads(nt) {}
};

int main() {
  ThreadPool p1;
  ThreadPool p2(4);
}
```

```rust
# fn cpu_count() -> usize {
#     std::thread::available_parallelism().unwrap().get()
# }
#
struct ThreadPool {
  num_threads: usize
}

impl ThreadPool {
    fn new() -> Self {
        Self { num_threads: cpu_count() }
    }

    fn with_threads(nt: usize) -> Self {
        Self { num_threads: nt }
    }
}

fn main() {
    let p1 = ThreadPool::new();
    let p2 = ThreadPool::with_threads(4);
}
```

</div>


在Rust中，类型的默认构造函数通常命名为`new`，尤其当它不接收参数时，详见[默认构造函数](./constructors/default_constructors.html)章节。基于值/对象特定属性的构造函数通常采用`with_<属性名>`的命名形式，例如`ThreadPool::with_threads`。关于Rust构造函数方法的命名规范，请参阅[命名指南](https://rust-lang.github.io/api-guidelines/naming.html)。

若需初始化的属性可见（译者注：即 `public`）、存在合理的默认值且该值不涉及资源管理，则通常也会基于某个默认值，采用记录更新语法来完成初始化。

```rust
struct Point {
    x: i32,
    y: i32,
    z: i32,
}

impl Point {
    const fn zero() -> Self {
        Self { x: 0, y: 0, z: 0 }
    }
}

fn main() {
    let x_unit = Point {
        x: 1,
        ..Point::zero()
    };
}
```

尽管名为“记录更新语法”，但它并不修改记录本身，而是基于现有值创建一个新值，并在此过程中取得所有权。

## 内存分配 vs 初始化

在Rust中，结构体或枚举值的实际构造发生在结构体构造语法（例如`ThreadPool { ... }`）所在位置，且在所有字段表达式（如`cpu_count()`）求值完成之后。

这一差异的重要含义在于：在Rust中，结构体的内存分配并非发生在其构造方法（例如`ThreadPool::with_threads`）被调用的时候，实际上要等到结构体所有字段的值都计算完成后才会分配内存（根据语言语义而言——优化器仍可能避免复制操作）。因此，Rust中没有直接对应 C++ 诸如"在构造时存储指向自身指针的类"这些模式的功能（在Rust中实现此类功能需要借助[`Pin`](https://doc.rust-lang.org/std/pin/struct.Pin.html)和[`MaybeUninit`](https://doc.rust-lang.org/std/mem/union.MaybeUninit.html)等工具）。

## 可以指示失败的构造函数

在C++中，构造函数主要通过抛出异常来指示失败。而在Rust中，由于构造函数是普通的静态方法，可失败的构造函数可以返回 `Result`（类似于`std::expected`）或`Option`（类似于`std::optional`）。[^NonZero]

[^NonZero]: 此处另一种方法是使用`NonZero<usize>`作为类型，这样从一开始就避免了错误情况的发生。

<div class="comparison">

```cpp
#include <iostream>
#include <stdexcept>

class ThreadPool {
  unsigned int num_threads;

public:
  ThreadPool(unsigned int nt) : num_threads(nt) {
    if (num_threads == 0) {
      throw std::domain_error(
          "Cannot have zero threads");
    }
  }
};

int main() {
  try {
    ThreadPool p(0);
    // use p here
  } catch (const std::domain_error &e) {
    std::cout << e.what() << std::endl;
  }
}
```

```rust
struct ThreadPool {
    num_threads: usize,
}

#[derive(Debug)]
enum ThreadPoolError {
    ZeroThreads,
}

impl ThreadPool {
    fn with_threads(
        nt: usize,
    ) -> Result<Self, ThreadPoolError> {
        if nt == 0 {
            Err(ThreadPoolError::ZeroThreads)
        } else {
            Ok(Self { num_threads: nt })
        }
    }
}

fn main() {
    match ThreadPool::with_threads(0) {
        Err(err) => println!("{:?}", err),
        Ok(p) => {
            // use p here
        }
    }
}
```

</div>

有关C++异常及其异常处理如何对应到Rust的更多信息，请参阅[异常章节](./exceptions.md)。
