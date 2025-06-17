# Summary

[C++ 到 Rust 速查手册](./title-page.md)

# 语法

- [构造函数](./idioms/constructors.md)
  - [默认构造函数](./idioms/constructors/default_constructors.md)
  - [拷贝和移动构造函数](./idioms/constructors/copy_and_move_constructors.md)
  - [3/5/0 规则](./idioms/constructors/rule_of_three_five_zero.md)
  <!-- - [Separate construction and initialization](./idioms/constructors/partial_initialzation.md) -->
- [析构函数和资源清理](./idioms/destructors.md)
- [数据建模](./idioms/data_modeling.md)
  - [抽象类、接口和动态派发](./idioms/data_modeling/abstract_classes.md)
  - [概念、接口和静态派发](./idioms/data_modeling/concepts.md)
  - [枚举](./idioms/data_modeling/enums.md)
  - [联合体和 `std::variant`](./idioms/data_modeling/tagged_unions.md)
  - [继承和实现重用](./idioms/data_modeling/inheritance_and_reuse.md)
  - [模板类、函数和方法](./idioms/data_modeling/templates.md)
  - [模板特化](./idioms/data_modeling/template_specialization.md)
- [Null (`nullptr`)](./idioms/null.md)
  - [哨兵值](./idioms/null/sentinel_values.md)
  - [移动成员](./idioms/null/moved_members.md)
  - [零长度数组](./idioms/null/zero_length_arrays.md)
- [封装](./idioms/encapsulation.md)
  - [头文件](./idioms/encapsulation/headers.md)
  - [匿名命名空间和 `static`](./idioms/encapsulation/anonymous_namespaces.md)
  - [私有成员和友元](./idioms/encapsulation/private_and_friends.md)
  - [私有构造函数](./idioms/encapsulation/private_constructors.md)
  - [Setter 和 getter 方法](./idioms/encapsulation/setters_and_getters.md)
- [异常和错误处理](./idioms/exceptions.md)
  - [预期错误](./idioms/exceptions/expected_errors.md)
  - [指示错误的 bug](./idioms/exceptions/bugs.md)
- [类型等价](./idioms/type_equivalents.md)
- [类型提升和转换](./idioms/promotions_and_conversions.md)
- [用户定义的转换](./idioms/user-defined_conversions.md)
- [重载](./idioms/overloading.md)
- [RTTI 和 `dynamic_cast`](./idioms/rtti.md)
- [迭代器](./idioms/iterators.md)
- [函数对象、lambda 和闭包](./idioms/function_objects.md)
- [对象标识](./idioms/object_identity.md)
- [输出参数](./idioms/out_params.md)
  - [多个返回值](./idioms/out_params/multiple_return.md)
  - [可选返回值](./idioms/out_params/optional_return.md)
  - [预分配的缓冲区](./idioms/out_params/pre-allocated_buffers.md)
- [可变参数](./idioms/varargs.md)
- [属性](./idioms/attributes.md)
- [调用 C (FFI)](./idioms/calling_c.md)
- [NRVO、RVO 和 placement new](./idioms/nrvo_rvo_and_placement_new.md)
- [并发 (线程和异步)](./idioms/concurrency.md)

# 模式

- [适配器模式](./patterns/adapter.md)
- [访问者模式和双分派](./patterns/visitor.md)
- [奇异递归模板模式 (CRTP)](./patterns/crtp.md)
- [指向实现的指针 (PImpl)](./patterns/pimpl.md)
- [X 宏]()

# 生态

- [库](./etc/libraries.md)
- [单元测试](./etc/unit_tests.md)
- [文档 (Doxygen)](./etc/doxygen.md)
- [构建系统 (CMake)](./etc/cmake.md)

---

- [贡献指南](./notices.md)
