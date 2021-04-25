# Node.js 在 MacBook Pro M1 环境的错误


<!--more-->

最近换了新的`MacBook Pro M1`开发，遇到了一些 Node.js 运行错误，都是一些`M1`芯片下比较常见的问题：
1. 一个旧项目运行出现了`out of memory`的错误：
    ```
    <--- Last few GCs --->

    [21371:0x128008000]     1982 ms: Scavenge 61.4 (90.6) -> 53.1 (90.6) MB, 9.7 / 0.0 ms  (average mu = 0.989, current mu = 0.989) task 


    <--- JS stacktrace --->

    FATAL ERROR: wasm code commit Allocation failed - process out of memory
    ```

    ![node out of memory](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/node-out-of-memory.png)

    > 环境
    > * Node.js: v14.16.1
    > * OS: macOS Big Sur with an M1 chip

    参考[此处](https://github.com/TypeStrong/typedoc/issues/1491)，升级至`Node.js v15`解决了此问题。

2. 使用`create-react-app`新建的项目首次运行出现了`Check failed: allocator->SetPermissions`
    ```
    #
    # Fatal error in , line 0
    # Check failed: allocator->SetPermissions(reinterpret_cast<void*>   (region.begin()), region.size(), PageAllocator::kNoAccess).
    #
    #
    #
    #FailureMessage Object: 0x16f4c8568
    1: 0x100a33944 node::NodePlatform::GetStackTracePrinter()   ::$_3::__invoke() [/Users/jiangbao/.nvm/versions/node/v15.3.0/   bin/node]
    2: 0x1017635e8 V8_Fatal(char const*, ...) [/Users/jiangbao/.  nvm/ versions/node/v15.3.0/bin/node]
    3: 0x1010c8d50 v8::internal::wasm::WasmCodeManager::Decommit  (v8::base::AddressRegion) [/Users/jiangbao/.nvm/versions/node/   v15.3.0/bin/node]
    4: 0x1010cc3d0 v8::internal::wasm::NativeModule::FreeCode   (v8::internal::Vector<v8::internal::wasm::WasmCode* const>) [/   Users/jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    5: 0x1010db5b8  v8::internal::wasm::WasmEngine::FreeDeadCodeLocked   (std::__1::unordered_map<v8::internal::wasm::NativeModule*,    std::__1::vector<v8::internal::wasm::WasmCode*,    std::__1::allocator<v8::internal::wasm::WasmCode*> >,    std::__1::hash<v8::internal::wasm::NativeModule*>,   std::__1::equal_to<v8::internal::wasm::NativeModule*>,     std::__1::allocator<std::__1::pair<v8::internal::wasm::NativeMod  u le* const, std::__1::vector<v8::internal::wasm::WasmCode*,    std::__1::allocator<v8::internal::wasm::WasmCode*> > > > >   const& ) [/Users/jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    6: 0x1010d92c0    v8::internal::wasm::WasmEngine::PotentiallyFinishCurrentGC() [/  Users/jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    7: 0x1010da734    v8::internal::wasm::WasmEngine::ReportLiveCodeForGC  (v8::internal::Isolate*,     v8::internal::Vector<v8::internal::wasm::WasmCode*>) [/Users/   jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    8: 0x1010daae4    v8::internal::wasm::WasmEngine::ReportLiveCodeFromStackForGC   (v8::internal::Isolate*) [/Users/jiangbao/.nvm/versions/node/  v15. 3.0/bin/node]
    9: 0x100c64560 v8::internal::StackGuard::HandleInterrupts() [/  Users/jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    10: 0x100f9f9e8 v8::internal::Runtime_StackGuard(int, unsigned long*, v8::internal::Isolate*) [/Users/jiangbao/.nvm/versions/node/v15.3.0/bin/node]
    ```

    > 环境
    > * Node.js: v15.3.0
    > * OS: macOS Big Sur with an M1 chip

    看到[这里](https://github.com/nodejs/node/issues/37061)指出在 v15.9.0 修复了此问题，干脆直接升级到最新发布的 v16，成功解决了此问题
