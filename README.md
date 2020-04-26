<p align="center"> 
  <img src="https://i.imgur.com/s3r4mxS.png" alt="alt logo">
</p>

[![PayPal](https://github.com/Rageware/Shields/blob/master/paypal.svg)](https://www.paypal.me/nxrighthere) [![Bountysource](https://github.com/Rageware/Shields/blob/master/bountysource.svg)](https://salt.bountysource.com/checkout/amount?team=nxrighthere) [![Coinbase](https://github.com/Rageware/Shields/blob/master/coinbase.svg)](https://commerce.coinbase.com/checkout/03e11816-b6fc-4e14-b974-29a1d0886697)

This repository provides a native, single-header, C API of the [Unigine](https://unigine.com) engine transpiled from managed code for bindings in programming languages such as Rust, Zig, Nim, and many others. It can be used with any C-compatible compiler such as GCC, Clang, or MSVC. The header doesn't provide any high-level abstractions of the API and lacks opaque pointers for a better type-safety.

Usage
--------
Create a new or use an existing C++ project, recreate `source` folder with C sources, copy `UnigineWrapper_x64.dll` from `SDK\bin` directory to the `bin` directory of the project. Compile an executable by linking against `UnigineWrapper_x64.dll` library. Run the executable.



##### Minimal executable:
```c
#include "unigine.h"

int main(int argumentsCount, char** arguments) {
	void* engine = unigine_engine_initialize(UNIGINE_VERSION, "Game", NULL, NULL, argumentsCount, arguments, NULL, NULL);

	unigine_engine_main(engine, NULL, NULL, NULL);
	unigine_engine_deinitialize();
}
```
