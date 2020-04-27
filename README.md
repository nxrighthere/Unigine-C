<p align="center"> 
  <img src="https://i.imgur.com/s3r4mxS.png" alt="alt logo">
</p>

[![PayPal](https://github.com/Rageware/Shields/blob/master/paypal.svg)](https://www.paypal.me/nxrighthere) [![Bountysource](https://github.com/Rageware/Shields/blob/master/bountysource.svg)](https://salt.bountysource.com/checkout/amount?team=nxrighthere) [![Coinbase](https://github.com/Rageware/Shields/blob/master/coinbase.svg)](https://commerce.coinbase.com/checkout/03e11816-b6fc-4e14-b974-29a1d0886697)

This repository provides a single-header, native C API of the [Unigine](https://unigine.com) engine transpiled from managed code for bindings in programming languages such as Rust, Zig, Nim, and many others. It can be used with any C-compatible compiler such as GCC, Clang, or MSVC. The header doesn't provide any high-level abstractions of the API and lacks opaque pointers for type-safety. Out of the box, Unigine supports C++ and C# languages, consider learning API from there.

Usage
--------
Create a new or use an existing C++ project, recreate `source` folder with C sources, copy `UnigineWrapper_x64.dll` from `SDK\bin` directory to the `bin` directory of the project. Compile an executable by linking against `UnigineWrapper_x64.dll` library. Run the executable. Batch files in the root directory of the project can be edited accordingly.

The engine API can be used in two ways: by using unformatted imported functions directly, or by using macro aliases that follow C naming conventions.

##### Minimal executable:
```c
#include "unigine.h"

int main(int argumentsCount, char** arguments) {
	void* engine = unigine_engine_initialize(UNIGINE_VERSION, "Game", NULL, NULL, argumentsCount, arguments, NULL, NULL);

	unigine_engine_main(engine, NULL, NULL, NULL);
	unigine_engine_deinitialize();
}
```

##### Basic systems:
```c
#include "unigine.h"

// System logic

int game_system_logic_start(void) {
	/* Write here code to be called on engine initialization */

	unigine_console_run("show_messages 1");
	unigine_log_message("Hello, Unigine from C!\n");

	return 1;
}

int game_system_logic_destroy(void) {
	/* Write here code to be called on engine shutdown */

	return 1;
}

int game_system_logic_destroy_render_resources(void) {
	/* Write here code to be called when the video mode is changed or application is restarted */

	return 1;
}

int game_system_logic_update(void) {
	/* Write here code to be called before updating each render frame */

	return 1;
}

int game_system_logic_post_update(void) {
	/* Write here code to be called after updating each render frame */

	return 1;
}

// World logic

int game_world_logic_start(void) {
	/* Write here code to be called on world initialization: initialize resources for your world scene during the world start */

	return 1;
}

int game_world_logic_destroy(void) {
	/* Write here code to be called on world shutdown: delete resources that were created during world script execution to avoid memory leaks */

	return 1;
}

int game_world_logic_destroy_render_resources(void) {
	/* Write here code to be called when the video mode is changed or application is restarted */

	return 1;
}

void game_world_logic_update_async_thread(int32_t id, int32_t size) {
	/* Write here code to be called after execution of all update sync thread functions */
}

void game_world_logic_update_sync_thread(int32_t id, int32_t size) {
	/* Write here code to be called before the update and the post-update functions */
}

int game_world_logic_update(void) {
	/* Write here code to be called before updating each render frame: specify all graphics-related functions you want to be called every frame while your application executes */

	return 1;
}

int game_world_logic_post_update(void) {
	/* Write here code to be called after updating each render frame: correct behavior after the state of the node has been updated */

	return 1;
}

int game_world_logic_update_physics(void) {
	/* Write here code to be called before updating each physics frame: control physics in your application and put non-rendering calculations */

	return 1;
}

int game_world_logic_swap(void) {
	/* Write here code to be called before updating each render frame */

	return 1;
}

int game_world_logic_save(const void* stream) {
	/* Write here code to be called when the world is saving its state, save custom user data to a file */

	return 1;
}

int game_world_logic_restore(const void* stream) {
	/* Write here code to be called when the world is restoring its state, restore custom user data to a file here */

	return 1;
}

int main(int argumentsCount, char** arguments) {
	void* engine = unigine_engine_initialize(UNIGINE_VERSION, "Game", NULL, NULL, argumentsCount, arguments, NULL, NULL);

	void* systemFunctions[5] = {
		&game_system_logic_start,
		&game_system_logic_destroy,
		&game_system_logic_destroy_render_resources,
		&game_system_logic_update,
		&game_system_logic_post_update
	};

	void* worldFunctions[11] = {
		&game_world_logic_start,
		&game_world_logic_destroy,
		&game_world_logic_destroy_render_resources,
		&game_world_logic_update_async_thread,
		&game_world_logic_update_sync_thread,
		&game_world_logic_update,
		&game_world_logic_post_update,
		&game_world_logic_update_physics,
		&game_world_logic_swap,
		&game_world_logic_save,
		&game_world_logic_restore
	};

	void* systemLogic = unigine_system_logic_construct(systemFunctions);
	void* worldLogic = unigine_world_logic_construct(worldFunctions);

	unigine_engine_main(engine, systemLogic, worldLogic, NULL);
	unigine_system_logic_destruct(systemLogic);
	unigine_world_logic_destruct(worldLogic);
	unigine_engine_deinitialize();
}
```
