Configuration Files

Ce contenu n’est pas encore disponible dans votre langue.

Since Tauri is a toolkit for building applications there can be many files to configure project settings. Some common files that you may run across are tauri.conf.json, package.json and Cargo.toml. We briefly explain each on this page to help point you in the right direction for which files to modify.
Tauri Config

The Tauri configuration is used to define the source of your Web app, describe your application’s metadata, configure bundles, set plugin configurations, modify runtime behavior by configuring windows, tray icons, menus and more.

This file is used by the Tauri runtime and the Tauri CLI. You can define build settings (such as the command run before tauri build or tauri dev kicks in), set the name and version of your app, control the Tauri runtime, and configure plugins.

Tip

You can find all of the options in the configuration reference.
Supported Formats

The default Tauri config format is JSON. The JSON5 or TOML format can be enabled by adding the config-json5 or config-toml feature flag (respectively) to the tauri and tauri-build dependencies in Cargo.toml.
Cargo.toml

[build-dependencies]
tauri-build = { version = "2.0.0", features = [ "config-json5" ] }

[dependencies]
tauri = { version = "2.0.0", features = [  "config-json5" ] }

The structure and values are the same across all formats, however, the formatting should be consistent with the respective file’s format:
tauri.conf.json

{
  build: {
    devUrl: 'http://localhost:3000',
    // start the dev server
    beforeDevCommand: 'npm run dev',
  },
  bundle: {
    active: true,
    icon: ['icons/app.png'],
  },
  app: {
    windows: [
      {
        title: 'MyApp',
      },
    ],
  },
  plugins: {
    updater: {
      pubkey: 'updater pub key',
      endpoints: ['https://my.app.updater/{{target}}/{{current_version}}'],
    },
  },
}

Tauri.toml

[build]
dev-url = "http://localhost:3000"
# start the dev server
before-dev-command = "npm run dev"

[bundle]
active = true
icon = ["icons/app.png"]

[[app.windows]]
title = "MyApp"

[plugins.updater]
pubkey = "updater pub key"
endpoints = ["https://my.app.updater/{{target}}/{{current_version}}"]

Note that JSON5 and TOML supports comments, and TOML can use kebab-case for config names which are more idiomatic.
Platform-specific Configuration

In addition to the default configuration file, Tauri can read a platform-specific configuration from:

    tauri.linux.conf.json or Tauri.linux.toml for Linux
    tauri.windows.conf.json or Tauri.windows.toml for Windows
    tauri.macos.conf.json or Tauri.macos.toml for macOS
    tauri.android.conf.json or Tauri.android.toml for Android
    tauri.ios.conf.json or Tauri.ios.toml for iOS

The platform-specific configuration file gets merged with the main configuration object following the JSON Merge Patch (RFC 7396) specification.

For example, given the following base tauri.conf.json:
tauri.conf.json

{
  "productName": "MyApp",
  "bundle": {
    "resources": ["./resources"]
  },
  "plugins": {
    "deep-link": {}
  }
}

And the given tauri.linux.conf.json:
tauri.linux.conf.json

{
  "productName": "my-app",
  "bundle": {
    "resources": ["./linux-assets"]
  },
  "plugins": {
    "cli": {
      "description": "My app",
      "subcommands": {
        "update": {}
      }
    },
    "deep-link": {}
  }
}

The resolved configuration for Linux would be the following object:

{
  "productName": "my-app",
  "bundle": {
    "resources": ["./linux-assets"]
  },
  "plugins": {
    "cli": {
      "description": "My app",
      "subcommands": {
        "update": {}
      }
    },
    "deep-link": {}
  }
}

Additionally you can provide a configuration to be merged via the CLI, see the following section for more information.
Extending the Configuration

The Tauri CLI allows you to extend the Tauri configuration when running one of the dev, android dev, ios dev, build, android build, ios build or bundle commands. The configuration extension can be provided by the --config argument either as a raw JSON string or as a path to a JSON file. Tauri uses the JSON Merge Patch (RFC 7396) specification to merge the provided configuration value with the originally resolved configuration object.

This mechanism can be used to define multiple flavours of your application or have more flexibility when configuring your application bundles.

For instance to distribute a completely isolated beta application you can use this feature to configure a separate application name and identifier:
src-tauri/tauri.beta.conf.json

{
  "productName": "My App Beta",
  "identifier": "com.myorg.myappbeta"
}

And to distribute this separate beta app you provide this configuration file when building it:

    npm
    yarn
    pnpm
    deno
    bun
    cargo

cargo tauri build --config src-tauri/tauri.beta.conf.json

Cargo.toml

Cargo’s manifest file is used to declare Rust crates your app depends on, metadata about your app, and other Rust-related features. If you do not intend to do backend development using Rust for your app then you may not be modifying it much, but it’s important to know that it exists and what it does.

Below is an example of a barebones Cargo.toml file for a Tauri project:
Cargo.toml

[package]
name = "app"
version = "0.1.0"
description = "A Tauri App"
authors = ["you"]
license = ""
repository = ""
default-run = "app"
edition = "2021"
rust-version = "1.57"

[build-dependencies]
tauri-build = { version = "2.0.0" }

[dependencies]
serde_json = "1.0"
serde = { version = "1.0", features = ["derive"] }
tauri = { version = "2.0.0", features = [ ] }

The most important parts to take note of are the tauri-build and tauri dependencies. Generally, they must both be on the latest minor versions as the Tauri CLI, but this is not strictly required. If you encounter issues while trying to run your app you should check that any Tauri versions (tauri and tauri-cli) are on the latest versions for their respective minor releases.

Cargo version numbers use Semantic Versioning. Running cargo update in the src-tauri folder will pull the latest available Semver-compatible versions of all dependencies. For example, if you specify 2.0.0 as the version for tauri-build, Cargo will detect and download version 2.0.0.0 because it is the latest Semver-compatible version available. Tauri will update the major version number whenever a breaking change is introduced, meaning you should always be capable of safely upgrading to the latest minor and patch versions without fear of your code breaking.

If you want to use a specific crate version you can use exact versions instead by prepending = to the version number of the dependency:

tauri-build = { version = "=2.0.0" }

An additional thing to take note of is the features=[] portion of the tauri dependency. Running tauri dev and tauri build will automatically manage which features need to be enabled in your project based on the your Tauri configuration. For more information about tauri feature flags see the documentation.

When you build your application a Cargo.lock file is produced. This file is used primarily for ensuring that the same dependencies are used across machines during development (similar to yarn.lock, pnpm-lock.yaml or package-lock.json in Node.js). It is recommended to commit this file to your source repository so you get consistent builds.

To learn more about the Cargo manifest file please refer to the official documentation.
package.json

This is the package file used by Node.js. If the frontend of your Tauri app is developed using Node.js-based technologies (such as npm, yarn, or pnpm) this file is used to configure the frontend dependencies and scripts.

An example of a barebones package.json file for a Tauri project might look a little something like this:
package.json

{
  "scripts": {
    "dev": "command to start your app development mode",
    "build": "command to build your app frontend",
    "tauri": "tauri"
  },
  "dependencies": {
    "@tauri-apps/api": "^2.0.0.0",
    "@tauri-apps/cli": "^2.0.0.0"
  }
}

It’s common to use the "scripts" section to store the commands used to launch and build the frontend used by your Tauri application. The above package.json file specifies the dev command that you can run using yarn dev or npm run dev to start the frontend framework and the build command that you can run using yarn build or npm run build to build your frontend’s Web assets to be added by Tauri in production. The most convenient way to use these scripts is to hook them with the Tauri CLI via the Tauri configuration’s beforeDevCommand and beforeBuildCommand hooks:
tauri.conf.json

{
  "build": {
    "beforeDevCommand": "yarn dev",
    "beforeBuildCommand": "yarn build"
  }
}

Note

The "tauri" script is only needed when using npm

The dependencies object specifies which dependencies Node.js should download when you run either yarn, pnpm install or npm install (in this case the Tauri CLI and API).

In addition to the package.json file you may see either a yarn.lock, pnpm-lock.yaml or package-lock.json file. These files assist in ensuring that when you download the dependencies later you’ll get the exact same versions that you have used during development (similar to Cargo.lock in Rust).

To learn more about the package.json file format please refer to the official documentation.
Calling Rust from the Frontend

Ce contenu n’est pas encore disponible dans votre langue.

This document includes guides on how to communicate with your Rust code from your application frontend. To see how to communicate with your frontend from your Rust code, see Calling the Frontend from Rust.

Tauri provides a command primitive for reaching Rust functions with type safety, along with an event system that is more dynamic.
Commands

Tauri provides a simple yet powerful command system for calling Rust functions from your web app. Commands can accept arguments and return values. They can also return errors and be async.
Basic Example

Commands can be defined in your src-tauri/src/lib.rs file. To create a command, just add a function and annotate it with #[tauri::command]:
src-tauri/src/lib.rs

#[tauri::command]
fn my_custom_command() {
  println!("I was invoked from JavaScript!");
}

Note

Command names must be unique.

Note

Commands defined in the lib.rs file cannot be marked as pub due to a limitation in the glue code generation. You will see an error like this if you mark it as a public function:

error[E0255]: the name `__cmd__command_name` is defined multiple times
  --> src/lib.rs:28:8
   |
27 | #[tauri::command]
   | ----------------- previous definition of the macro `__cmd__command_name` here
28 | pub fn x() {}
   |        ^ `__cmd__command_name` reimported here
   |
   = note: `__cmd__command_name` must be defined only once in the macro namespace of this module
```

You will have to provide a list of your commands to the builder function like so:
src-tauri/src/lib.rs

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![my_custom_command])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

Now, you can invoke the command from your JavaScript code:

// When using the Tauri API npm package:
import { invoke } from '@tauri-apps/api/core';

// When using the Tauri global script (if not using the npm package)
// Be sure to set `app.withGlobalTauri` in `tauri.conf.json` to true
const invoke = window.__TAURI__.core.invoke;

// Invoke the command
invoke('my_custom_command');

Defining Commands in a Separate Module

If your application defines a lot of components or if they can be grouped, you can define commands in a separate module instead of bloating the lib.rs file.

As an example let’s define a command in the src-tauri/src/commands.rs file:
src-tauri/src/commands.rs

#[tauri::command]
pub fn my_custom_command() {
  println!("I was invoked from JavaScript!");
}

Note

When defining commands in a separate module they should be marked as pub.

Note

The command name is not scoped to the module so they must be unique even between modules.

In the lib.rs file, define the module and provide the list of your commands accordingly;
src-tauri/src/lib.rs

mod commands;

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![commands::my_custom_command])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

Note the commands:: prefix in the command list, which denotes the full path to the command function.

The command name in this example is my_custom_command so you can still call it by executing invoke("my_custom_command") in your frontend, the commands:: prefix is ignored.
WASM

When using a Rust frontend to call invoke() without arguments, you will need to adapt your frontend code as below. The reason is that Rust doesn’t support optional arguments.

#[wasm_bindgen]
extern "C" {
    // invoke without arguments
    #[wasm_bindgen(js_namespace = ["window", "__TAURI__", "core"], js_name = invoke)]
    async fn invoke_without_args(cmd: &str) -> JsValue;

    // invoke with arguments (default)
    #[wasm_bindgen(js_namespace = ["window", "__TAURI__", "core"])]
    async fn invoke(cmd: &str, args: JsValue) -> JsValue;

    // They need to have different names!
}

Passing Arguments

Your command handlers can take arguments:

#[tauri::command]
fn my_custom_command(invoke_message: String) {
  println!("I was invoked from JavaScript, with this message: {}", invoke_message);
}

Arguments should be passed as a JSON object with camelCase keys:

invoke('my_custom_command', { invokeMessage: 'Hello!' });

Note

You can use snake_case for the arguments with the rename_all attribute:

#[tauri::command(rename_all = "snake_case")]
fn my_custom_command(invoke_message: String) {}

invoke('my_custom_command', { invoke_message: 'Hello!' });

Arguments can be of any type, as long as they implement serde::Deserialize.

The corresponding JavaScript:

invoke('my_custom_command', { invoke_message: 'Hello!' });

Returning Data

Command handlers can return data as well:

#[tauri::command]
fn my_custom_command() -> String {
  "Hello from Rust!".into()
}

The invoke function returns a promise that resolves with the returned value:

invoke('my_custom_command').then((message) => console.log(message));

Returned data can be of any type, as long as it implements serde::Serialize.
Returning Array Buffers

Return values that implements serde::Serialize are serialized to JSON when the response is sent to the frontend. This can slow down your application if you try to return a large data such as a file or a download HTTP response. To return array buffers in an optimized way, use tauri::ipc::Response:

use tauri::ipc::Response;
#[tauri::command]
fn read_file() -> Response {
  let data = std::fs::read("/path/to/file").unwrap();
  tauri::ipc::Response::new(data)
}

Error Handling

If your handler could fail and needs to be able to return an error, have the function return a Result:

#[tauri::command]
fn login(user: String, password: String) -> Result<String, String> {
  if user == "tauri" && password == "tauri" {
    // resolve
    Ok("logged_in".to_string())
  } else {
    // reject
    Err("invalid credentials".to_string())
  }
}

If the command returns an error, the promise will reject, otherwise, it resolves:

invoke('login', { user: 'tauri', password: '0j4rijw8=' })
  .then((message) => console.log(message))
  .catch((error) => console.error(error));

As mentioned above, everything returned from commands must implement serde::Serialize, including errors. This can be problematic if you’re working with error types from Rust’s std library or external crates as most error types do not implement it. In simple scenarios you can use map_err to convert these errors to Strings:

#[tauri::command]
fn my_custom_command() -> Result<(), String> {
  std::fs::File::open("path/to/file").map_err(|err| err.to_string())?;
  // Return `null` on success
  Ok(())
}

Since this is not very idiomatic you may want to create your own error type which implements serde::Serialize. In the following example, we use the thiserror crate to help create the error type. It allows you to turn enums into error types by deriving the thiserror::Error trait. You can consult its documentation for more details.

// create the error type that represents all errors possible in our program
#[derive(Debug, thiserror::Error)]
enum Error {
  #[error(transparent)]
  Io(#[from] std::io::Error)
}

// we must manually implement serde::Serialize
impl serde::Serialize for Error {
  fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
  where
    S: serde::ser::Serializer,
  {
    serializer.serialize_str(self.to_string().as_ref())
  }
}

#[tauri::command]
fn my_custom_command() -> Result<(), Error> {
  // This will return an error
  std::fs::File::open("path/that/does/not/exist")?;
  // Return `null` on success
  Ok(())
}

A custom error type has the advantage of making all possible errors explicit so readers can quickly identify what errors can happen. This saves other people (and yourself) enormous amounts of time when reviewing and refactoring code later.
It also gives you full control over the way your error type gets serialized. In the above example, we simply returned the error message as a string, but you could assign each error a code so you could more easily map it to a similar looking TypeScript error enum for example:

#[derive(Debug, thiserror::Error)]
enum Error {
  #[error(transparent)]
  Io(#[from] std::io::Error),
  #[error("failed to parse as string: {0}")]
  Utf8(#[from] std::str::Utf8Error),
}

#[derive(serde::Serialize)]
#[serde(tag = "kind", content = "message")]
#[serde(rename_all = "camelCase")]
enum ErrorKind {
  Io(String),
  Utf8(String),
}

impl serde::Serialize for Error {
  fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
  where
    S: serde::ser::Serializer,
  {
    let error_message = self.to_string();
    let error_kind = match self {
      Self::Io(_) => ErrorKind::Io(error_message),
      Self::Utf8(_) => ErrorKind::Utf8(error_message),
    };
    error_kind.serialize(serializer)
  }
}

#[tauri::command]
fn read() -> Result<Vec<u8>, Error> {
  let data = std::fs::read("/path/to/file")?;
  Ok(data)
}

In your frontend you now get a { kind: 'io' | 'utf8', message: string } error object:

type ErrorKind = {
  kind: 'io' | 'utf8';
  message: string;
};

invoke('read').catch((e: ErrorKind) => {});

Async Commands

Asynchronous commands are preferred in Tauri to perform heavy work in a manner that doesn’t result in UI freezes or slowdowns.

Note

Async commands are executed on a separate async task using async_runtime::spawn. Commands without the async keyword are executed on the main thread unless defined with #[tauri::command(async)].

If your command needs to run asynchronously, simply declare it as async.

Caution

You need to be careful when creating asynchronous functions using Tauri. Currently, you cannot simply include borrowed arguments in the signature of an asynchronous function. Some common examples of types like this are &str and State<'_, Data>. This limitation is tracked here: https://github.com/tauri-apps/tauri/issues/2533 and workarounds are shown below.

When working with borrowed types, you have to make additional changes. These are your two main options:

Option 1: Convert the type, such as &str to a similar type that is not borrowed, such as String. This may not work for all types, for example State<'_, Data>.

Example:

// Declare the async function using String instead of &str, as &str is borrowed and thus unsupported
#[tauri::command]
async fn my_custom_command(value: String) -> String {
  // Call another async function and wait for it to finish
  some_async_function().await;
  value
}

Option 2: Wrap the return type in a Result. This one is a bit harder to implement, but works for all types.

Use the return type Result<a, b>, replacing a with the type you wish to return, or () if you wish to return null, and replacing b with an error type to return if something goes wrong, or () if you wish to have no optional error returned. For example:

    Result<String, ()> to return a String, and no error.
    Result<(), ()> to return null.
    Result<bool, Error> to return a boolean or an error as shown in the Error Handling section above.

Example:

// Return a Result<String, ()> to bypass the borrowing issue
#[tauri::command]
async fn my_custom_command(value: &str) -> Result<String, ()> {
  // Call another async function and wait for it to finish
  some_async_function().await;
  // Note that the return value must be wrapped in `Ok()` now.
  Ok(format!(value))
}

Invoking from JavaScript

Since invoking the command from JavaScript already returns a promise, it works just like any other command:

invoke('my_custom_command', { value: 'Hello, Async!' }).then(() =>
  console.log('Completed!')
);

Channels

The Tauri channel is the recommended mechanism for streaming data such as streamed HTTP responses to the frontend. The following example reads a file and notifies the frontend of the progress in chunks of 4096 bytes:

use tokio::io::AsyncReadExt;

#[tauri::command]
async fn load_image(path: std::path::PathBuf, reader: tauri::ipc::Channel<&[u8]>) {
  // for simplicity this example does not include error handling
  let mut file = tokio::fs::File::open(path).await.unwrap();

  let mut chunk = vec![0; 4096];

  loop {
    let len = file.read(&mut chunk).await.unwrap();
    if len == 0 {
      // Length of zero means end of file.
      break;
    }
    reader.send(&chunk).unwrap();
  }
}

See the channels documentation for more information.
Accessing the WebviewWindow in Commands

Commands can access the WebviewWindow instance that invoked the message:
src-tauri/src/lib.rs

#[tauri::command]
async fn my_custom_command(webview_window: tauri::WebviewWindow) {
  println!("WebviewWindow: {}", webview_window.label());
}

Accessing an AppHandle in Commands

Commands can access an AppHandle instance:
src-tauri/src/lib.rs

#[tauri::command]
async fn my_custom_command(app_handle: tauri::AppHandle) {
  let app_dir = app_handle.path_resolver().app_dir();
  use tauri::GlobalShortcutManager;
  app_handle.global_shortcut_manager().register("CTRL + U", move || {});
}

Accessing Managed State

Tauri can manage state using the manage function on tauri::Builder. The state can be accessed on a command using tauri::State:
src-tauri/src/lib.rs

struct MyState(String);

#[tauri::command]
fn my_custom_command(state: tauri::State<MyState>) {
  assert_eq!(state.0 == "some state value", true);
}

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .manage(MyState("some state value".into()))
    .invoke_handler(tauri::generate_handler![my_custom_command])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

Accessing Raw Request

Tauri commands can also access the full tauri::ipc::Request object which includes the raw body payload and the request headers.

#[derive(Debug, thiserror::Error)]
enum Error {
  #[error("unexpected request body")]
  RequestBodyMustBeRaw,
  #[error("missing `{0}` header")]
  MissingHeader(&'static str),
}

impl serde::Serialize for Error {
  fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
  where
    S: serde::ser::Serializer,
  {
    serializer.serialize_str(self.to_string().as_ref())
  }
}

#[tauri::command]
fn upload(request: tauri::ipc::Request) -> Result<(), Error> {
  let tauri::ipc::InvokeBody::Raw(upload_data) = request.body() else {
    return Err(Error::RequestBodyMustBeRaw);
  };
  let Some(authorization_header) = request.headers().get("Authorization") else {
    return Err(Error::MissingHeader("Authorization"));
  };

  // upload...

  Ok(())
}

In the frontend you can call invoke() sending a raw request body by providing an ArrayBuffer or Uint8Array on the payload argument, and include request headers in the third argument:

const data = new Uint8Array([1, 2, 3]);
await __TAURI__.core.invoke('upload', data, {
  headers: {
    Authorization: 'apikey',
  },
});

Creating Multiple Commands

The tauri::generate_handler! macro takes an array of commands. To register multiple commands, you cannot call invoke_handler multiple times. Only the last call will be used. You must pass each command to a single call of tauri::generate_handler!.
src-tauri/src/lib.rs

#[tauri::command]
fn cmd_a() -> String {
  "Command a"
}
#[tauri::command]
fn cmd_b() -> String {
  "Command b"
}

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![cmd_a, cmd_b])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

Complete Example

Any or all of the above features can be combined:
src-tauri/src/lib.rs

struct Database;

#[derive(serde::Serialize)]
struct CustomResponse {
  message: String,
  other_val: usize,
}

async fn some_other_function() -> Option<String> {
  Some("response".into())
}

#[tauri::command]
async fn my_custom_command(
  window: tauri::Window,
  number: usize,
  database: tauri::State<'_, Database>,
) -> Result<CustomResponse, String> {
  println!("Called from {}", window.label());
  let result: Option<String> = some_other_function().await;
  if let Some(message) = result {
    Ok(CustomResponse {
      message,
      other_val: 42 + number,
    })
  } else {
    Err("No result".into())
  }
}

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .manage(Database {})
    .invoke_handler(tauri::generate_handler![my_custom_command])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

import { invoke } from '@tauri-apps/api/core';

// Invocation from JavaScript
invoke('my_custom_command', {
  number: 42,
})
  .then((res) =>
    console.log(`Message: ${res.message}, Other Val: ${res.other_val}`)
  )
  .catch((e) => console.error(e));

Event System

The event system is a simpler communication mechanism between your frontend and the Rust. Unlike commands, events are not type safe, are always async, cannot return values and only supports JSON payloads.
Global Events

To trigger a global event you can use the event.emit or the WebviewWindow#emit functions:

import { emit } from '@tauri-apps/api/event';
import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

// emit(eventName, payload)
emit('file-selected', '/path/to/file');

const appWebview = getCurrentWebviewWindow();
appWebview.emit('route-changed', { url: window.location.href });

Note

Global events are delivered to all listeners
Webview Event

To trigger an event to a listener registered by a specific webview you can use the event.emitTo or the WebviewWindow#emitTo functions:

import { emitTo } from '@tauri-apps/api/event';
import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

// emitTo(webviewLabel, eventName, payload)
emitTo('settings', 'settings-update-requested', {
  key: 'notification',
  value: 'all',
});

const appWebview = getCurrentWebviewWindow();
appWebview.emitTo('editor', 'file-changed', {
  path: '/path/to/file',
  contents: 'file contents',
});

Note

Webview-specific events are not triggered to regular global event listeners. To listen to any event you must provide the { target: { kind: 'Any' } } option to the event.listen function, which defines the listener to act as a catch-all for emitted events:

import { listen } from '@tauri-apps/api/event';
listen(
  'state-changed',
  (event) => {
    console.log('got state changed event', event);
  },
  {
    target: { kind: 'Any' },
  }
);

Listening to Events

The @tauri-apps/api NPM package offers APIs to listen to both global and webview-specific events.

    Listening to global events

    import { listen } from '@tauri-apps/api/event';

    type DownloadStarted = {
      url: string;
      downloadId: number;
      contentLength: number;
    };

    listen<DownloadStarted>('download-started', (event) => {
      console.log(
        `downloading ${event.payload.contentLength} bytes from ${event.payload.url}`
      );
    });

Listening to webview-specific events

import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

const appWebview = getCurrentWebviewWindow();
appWebview.listen<string>('logged-in', (event) => {
  localStorage.setItem('session-token', event.payload);
});

The listen function keeps the event listener registered for the entire lifetime of the application. To stop listening on an event you can use the unlisten function which is returned by the listen function:

import { listen } from '@tauri-apps/api/event';

const unlisten = await listen('download-started', (event) => {});
unlisten();

Note

Always use the unlisten function when your execution context goes out of scope such as when a component is unmounted.

When the page is reloaded or you navigate to another URL the listeners are unregistered automatically. This does not apply to a Single Page Application (SPA) router though.

Additionally Tauri provides a utility function for listening to an event exactly once:

import { once } from '@tauri-apps/api/event';
import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

once('ready', (event) => {});

const appWebview = getCurrentWebviewWindow();
appWebview.once('ready', () => {});

Note

Events emitted in the frontend also triggers listeners registed by these APIs. For more information see the Calling Rust from the Frontend documentation.
Listening to Events on Rust

Global and webview-specific events are also delivered to listeners registered in Rust.

    Listening to global events
    src-tauri/src/lib.rs

    use tauri::Listener;

    #[cfg_attr(mobile, tauri::mobile_entry_point)]
    pub fn run() {
      tauri::Builder::default()
        .setup(|app| {
          app.listen("download-started", |event| {
            if let Ok(payload) = serde_json::from_str::<DownloadStarted>(&event.payload()) {
              println!("downloading {}", payload.url);
            }
          });
          Ok(())
        })
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
    }

Listening to webview-specific events
src-tauri/src/lib.rs

use tauri::{Listener, Manager};

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .setup(|app| {
      let webview = app.get_webview_window("main").unwrap();
      webview.listen("logged-in", |event| {
        let session_token = event.data;
        // save token..
      });
      Ok(())
    })
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

The listen function keeps the event listener registered for the entire lifetime of the application. To stop listening on an event you can use the unlisten function:

// unlisten outside of the event handler scope:
let event_id = app.listen("download-started", |event| {});
app.unlisten(event_id);

// unlisten when some event criteria is matched
let handle = app.handle().clone();
app.listen("status-changed", |event| {
  if event.data == "ready" {
    handle.unlisten(event.id);
  }
});

Additionally Tauri provides a utility function for listening to an event exactly once:

app.once("ready", |event| {
  println!("app is ready");
});

In this case the event listener is immediately unregistered after its first trigger.

To learn how to listen to events and emit events from your Rust code, see the Rust Event System 
Calling the Frontend from Rust

Ce contenu n’est pas encore disponible dans votre langue.

This document includes guides on how to communicate with your application frontend from your Rust code. To see how to communicate with your Rust code from your frontend, see Calling Rust from the Frontend.

The Rust side of your Tauri application can call the frontend by leveraging the Tauri event system, using channels or directly evaluating JavaScript code.
Event System

Tauri ships a simple event system you can use to have bi-directional communication between Rust and your frontend.

The event system was designed for situations where small amounts of data need to be streamed or you need to implement a multi consumer multi producer pattern (e.g. push notification system).

The event system is not designed for low latency or high throughput situations. See the channels section for the implementation optimized for streaming data.

The major differences between a Tauri command and a Tauri event are that events have no strong type support, event payloads are always JSON strings making them not suitable for bigger messages and there is no support of the capabilities system to fine grain control event data and channels.

The AppHandle and WebviewWindow types implement the event system traits Listener and Emitter.

Events are either global (delivered to all listeners) or webview-specific (only delivered to the webview matching a given label).
Global Events

To trigger a global event you can use the Emitter#emit function:
src-tauri/src/lib.rs

use tauri::{AppHandle, Emitter};

#[tauri::command]
fn download(app: AppHandle, url: String) {
  app.emit("download-started", &url).unwrap();
  for progress in [1, 15, 50, 80, 100] {
    app.emit("download-progress", progress).unwrap();
  }
  app.emit("download-finished", &url).unwrap();
}

Note

Global events are delivered to all listeners
Webview Event

To trigger an event to a listener registered by a specific webview you can use the Emitter#emit_to function:
src-tauri/src/lib.rs

use tauri::{AppHandle, Emitter};

#[tauri::command]
fn login(app: AppHandle, user: String, password: String) {
  let authenticated = user == "tauri-apps" && password == "tauri";
  let result = if authenticated { "loggedIn" } else { "invalidCredentials" };
  app.emit_to("login", "login-result", result).unwrap();
}

It is also possible to trigger an event to a list of webviews by calling Emitter#emit_filter. In the following example we emit a open-file event to the main and file-viewer webviews:
src-tauri/src/lib.rs

use tauri::{AppHandle, Emitter, EventTarget};

#[tauri::command]
fn open_file(app: AppHandle, path: std::path::PathBuf) {
  app.emit_filter("open-file", path, |target| match target {
    EventTarget::WebviewWindow { label } => label == "main" || label == "file-viewer",
    _ => false,
  }).unwrap();
}

Note

Webview-specific events are not triggered to regular global event listeners. To listen to any event you must use the listen_any function instead of listen, which defines the listener to act as a catch-all for emitted events.
Event Payload

The event payload can be any serializable type that also implements Clone. Let’s enhance the download event example by using an object to emit more information in each event:
src-tauri/src/lib.rs

use tauri::{AppHandle, Emitter};
use serde::Serialize;

#[derive(Clone, Serialize)]
#[serde(rename_all = "camelCase")]
struct DownloadStarted<'a> {
  url: &'a str,
  download_id: usize,
  content_length: usize,
}

#[derive(Clone, Serialize)]
#[serde(rename_all = "camelCase")]
struct DownloadProgress {
  download_id: usize,
  chunk_length: usize,
}

#[derive(Clone, Serialize)]
#[serde(rename_all = "camelCase")]
struct DownloadFinished {
  download_id: usize,
}

#[tauri::command]
fn download(app: AppHandle, url: String) {
  let content_length = 1000;
  let download_id = 1;

  app.emit("download-started", DownloadStarted {
    url: &url,
    download_id,
    content_length
  }).unwrap();

  for chunk_length in [15, 150, 35, 500, 300] {
    app.emit("download-progress", DownloadProgress {
      download_id,
      chunk_length,
    }).unwrap();
  }

  app.emit("download-finished", DownloadFinished { download_id }).unwrap();
}

Listening to Events

Tauri provides APIs to listen to events on both the webview and the Rust interfaces.
Listening to Events on the Frontend

The @tauri-apps/api NPM package offers APIs to listen to both global and webview-specific events.

    Listening to global events

    import { listen } from '@tauri-apps/api/event';

    type DownloadStarted = {
      url: string;
      downloadId: number;
      contentLength: number;
    };

    listen<DownloadStarted>('download-started', (event) => {
      console.log(
        `downloading ${event.payload.contentLength} bytes from ${event.payload.url}`
      );
    });

Listening to webview-specific events

import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

const appWebview = getCurrentWebviewWindow();
appWebview.listen<string>('logged-in', (event) => {
  localStorage.setItem('session-token', event.payload);
});

The listen function keeps the event listener registered for the entire lifetime of the application. To stop listening on an event you can use the unlisten function which is returned by the listen function:

import { listen } from '@tauri-apps/api/event';

const unlisten = await listen('download-started', (event) => {});
unlisten();

Note

Always use the unlisten function when your execution context goes out of scope such as when a component is unmounted.

When the page is reloaded or you navigate to another URL the listeners are unregistered automatically. This does not apply to a Single Page Application (SPA) router though.

Additionally Tauri provides a utility function for listening to an event exactly once:

import { once } from '@tauri-apps/api/event';
import { getCurrentWebviewWindow } from '@tauri-apps/api/webviewWindow';

once('ready', (event) => {});

const appWebview = getCurrentWebviewWindow();
appWebview.once('ready', () => {});

Note

Events emitted in the frontend also triggers listeners registed by these APIs. For more information see the Calling Rust from the Frontend documentation.
Listening to Events on Rust

Global and webview-specific events are also delivered to listeners registered in Rust.

    Listening to global events
    src-tauri/src/lib.rs

    use tauri::Listener;

    #[cfg_attr(mobile, tauri::mobile_entry_point)]
    pub fn run() {
      tauri::Builder::default()
        .setup(|app| {
          app.listen("download-started", |event| {
            if let Ok(payload) = serde_json::from_str::<DownloadStarted>(&event.payload()) {
              println!("downloading {}", payload.url);
            }
          });
          Ok(())
        })
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
    }

Listening to webview-specific events
src-tauri/src/lib.rs

use tauri::{Listener, Manager};

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .setup(|app| {
      let webview = app.get_webview_window("main").unwrap();
      webview.listen("logged-in", |event| {
        let session_token = event.data;
        // save token..
      });
      Ok(())
    })
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

The listen function keeps the event listener registered for the entire lifetime of the application. To stop listening on an event you can use the unlisten function:

// unlisten outside of the event handler scope:
let event_id = app.listen("download-started", |event| {});
app.unlisten(event_id);

// unlisten when some event criteria is matched
let handle = app.handle().clone();
app.listen("status-changed", |event| {
  if event.data == "ready" {
    handle.unlisten(event.id);
  }
});

Additionally Tauri provides a utility function for listening to an event exactly once:

app.once("ready", |event| {
  println!("app is ready");
});

In this case the event listener is immediately unregistered after its first trigger.
Channels

The event system is designed to be a simple two way communication that is globally available in your application. Under the hood it directly evaluates JavaScript code so it might not be suitable to sending a large amount of data.

Channels are designed to be fast and deliver ordered data. They are used internally for streaming operations such as download progress, child process output and WebSocket messages.

Let’s rewrite our download command example to use channels instead of the event system:
src-tauri/src/lib.rs

use tauri::{AppHandle, ipc::Channel};
use serde::Serialize;

#[derive(Clone, Serialize)]
#[serde(rename_all = "camelCase", rename_all_fields = "camelCase", tag = "event", content = "data")]
enum DownloadEvent<'a> {
  Started {
    url: &'a str,
    download_id: usize,
    content_length: usize,
  },
  Progress {
    download_id: usize,
    chunk_length: usize,
  },
  Finished {
    download_id: usize,
  },
}

#[tauri::command]
fn download(app: AppHandle, url: String, on_event: Channel<DownloadEvent>) {
  let content_length = 1000;
  let download_id = 1;

  on_event.send(DownloadEvent::Started {
    url: &url,
    download_id,
    content_length,
  }).unwrap();

  for chunk_length in [15, 150, 35, 500, 300] {
    on_event.send(DownloadEvent::Progress {
      download_id,
      chunk_length,
    }).unwrap();
  }

  on_event.send(DownloadEvent::Finished { download_id }).unwrap();
}

When calling the download command you must create the channel and provide it as an argument:

import { invoke, Channel } from '@tauri-apps/api/core';

type DownloadEvent =
  | {
      event: 'started';
      data: {
        url: string;
        downloadId: number;
        contentLength: number;
      };
    }
  | {
      event: 'progress';
      data: {
        downloadId: number;
        chunkLength: number;
      };
    }
  | {
      event: 'finished';
      data: {
        downloadId: number;
      };
    };

const onEvent = new Channel<DownloadEvent>();
onEvent.onmessage = (message) => {
  console.log(`got download event ${message.event}`);
};

await invoke('download', {
  url: 'https://raw.githubusercontent.com/tauri-apps/tauri/dev/crates/tauri-schema-generator/schemas/config.schema.json',
  onEvent,
});

Evaluating JavaScript

To directly execute any JavaScript code on the webview context you can use the WebviewWindow#eval function:
src-tauri/src/lib.rs

use tauri::Manager;

tauri::Builder::default()
  .setup(|app| {
    let webview = app.get_webview_window("main").unwrap();
    webview.eval("console.log('hello from Rust')")?;
    Ok(())
  })

If the script to be evaluated is not so simple and must use input from Rust objects we recommend using the serialize-to-javascript crate.
CrabNebula DevTools

Ce contenu n’est pas encore disponible dans votre langue.

CrabNebula provides a free DevTools application for Tauri as part of its partnership with the Tauri project. This application allows you to instrument your Tauri app by capturing its embedded assets, Tauri configuration file, logs and spans and providing a web frontend to seamlessly visualize data in real time.

With the CrabNebula DevTools you can inspect your app’s log events (including logs from dependencies), track down the performance of your command calls and overall Tauri API usage, with a special interface for Tauri events and commands, including payload, responses and inner logs and execution spans.

To enable the CrabNebula DevTools, install the devtools crate:

cargo add tauri-plugin-devtools@2.0.0

And initialize the plugin as soon as possible in your main function:

fn main() {
    // This should be called as early in the execution of the app as possible
    #[cfg(debug_assertions)] // only enable instrumentation in development builds
    let devtools = tauri_plugin_devtools::init();

    let mut builder = tauri::Builder::default();

    #[cfg(debug_assertions)]
    {
        builder = builder.plugin(devtools);
    }

    builder
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}

And then run your app as usual, if everything is set up correctly devtools will print the following message:
DevTools message on terminal

Note

In this case we only initialize the devtools plugin for debug applications, which is recommended.
What is Tauri?

Tauri is a framework for building tiny, fast binaries for all major desktop and mobile platforms. Developers can integrate any frontend framework that compiles to HTML, JavaScript, and CSS for building their user experience while leveraging languages such as Rust, Swift, and Kotlin for backend logic when needed.

Get started building with create-tauri-app by using one of the below commands. Be sure to follow the prerequisites guide to install all of the dependencies required by Tauri and then view the Frontend Configuration guides for recommended frontend configurations.

    Bash
    PowerShell
    Fish
    npm
    Yarn
    pnpm
    deno
    bun
    Cargo

cargo install create-tauri-app --locked
cargo create-tauri-app

After you’ve created your first app you can explore the different features and recipes of Tauri in the List of Features & Recipes.
Why Tauri?

Tauri has 3 main advantages for developers to build upon:

    Secure foundation for building apps
    Smaller bundle size by using the system’s native webview
    Flexibility for developers to use any frontend and bindings for multiple languages

Learn more about the Tauri philosophy in the Tauri 1.0 blog post.
Secure Foundation

By being built on Rust, Tauri is able to take advantage of the memory, thread, and type-safety offered by Rust. Apps built on Tauri can automatically get those benefits even without needing to be developed by Rust experts.

Tauri also undergoes a security audit for major and minor releases. This not only covers code in the Tauri organization, but also for upstream dependencies that Tauri relies on. Of course this doesn’t mitigate all risks, but it provides a solid foundation for developers to build on top of.

Read the Tauri security policy and the Tauri 2.0 audit report.
Smaller App Size

Tauri apps take advantage of the web view already available on every user’s system. A Tauri app only contains the code and assets specific for that app and doesn’t need to bundle a browser engine with every app. This means that a minimal Tauri app can be less than 600KB in size.

Learn more about creating optimized apps in the App Size concept.
Flexible Architecture

Since Tauri uses web technologies that means that virtually any frontend framework is compatible with Tauri. The Frontend Configuration guide contains common configurations for popular frontend frameworks.

Bindings between JavaScript and Rust are available to developers using the invoke function in JavaScript and Swift and Kotlin bindings are available for Tauri Plugins.

TAO is responsible for Tauri window creation and WRY is responsible for web view rendering. These are libraries maintained by Tauri and can be consumed directly if deeper system integration is required outside of what Tauri exposes.

In addition, Tauri maintains a number of plugins to extend what core Tauri exposes. You can find those plugins alongside those provided by the community in the Plugins section.



Prerequisites

In order to get started building your project with Tauri you’ll first need to install a few dependencies:

    System Dependencies
    Rust
    Configure for Mobile Targets (only required if developing for mobile)

System Dependencies

Follow the link to get started for your respective operating system:

    Linux (see below for specific distributions)
    macOS Catalina (10.15) and later
    Windows 7 and later

Linux

Tauri requires various system dependencies for development on Linux. These may be different depending on your distribution but we’ve included some popular distributions below to help you get setup.

    Debian
    Arch
    Fedora
    Gentoo
    openSUSE
    Alpine
    NixOS

Terminal window

sudo apt update
sudo apt install libwebkit2gtk-4.1-dev \
  build-essential \
  curl \
  wget \
  file \
  libxdo-dev \
  libssl-dev \
  libayatana-appindicator3-dev \
  librsvg2-dev

If your distribution isn’t included above then you may want to check Awesome Tauri on GitHub to see if a guide has been created.

Next: Install Rust
macOS

Tauri uses Xcode and various macOS and iOS development dependencies.

Download and install Xcode from one of the following places:

    Mac App Store
    Apple Developer website.

Be sure to launch Xcode after installing so that it can finish setting up.

Only developing for desktop targets?

Next: Install Rust
Windows

Tauri uses the Microsoft C++ Build Tools for development as well as Microsoft Edge WebView2. These are both required for development on Windows.

Follow the steps below to install the required dependencies.
Microsoft C++ Build Tools

    Download the Microsoft C++ Build Tools installer and open it to begin installation.
    During installation check the “Desktop development with C++” option.

Visual Studio C++ Build Tools installer screenshot

Next: Install WebView2.
WebView2

Tip

WebView 2 is already installed on Windows 10 (from version 1803 onward) and later versions of Windows. If you are developing on one of these versions then you can skip this step and go directly to installing Rust.

Tauri uses Microsoft Edge WebView2 to render content on Windows.

Install WebView2 by visiting the WebView2 Runtime download section. Download the “Evergreen Bootstrapper” and install it.

Next: Install Rust
Rust

Tauri is built with Rust and requires it for development. Install Rust using one of following methods. You can view more installation methods at https://www.rust-lang.org/tools/install.

    Linux and macOS
    Windows

Visit https://www.rust-lang.org/tools/install to install rustup.

Alternatively, you can use winget to install rustup using the following command in PowerShell:
Terminal window

winget install --id Rustlang.Rustup

MSVC toolchain as default

For full support for Tauri and tools like trunk make sure the MSVC Rust toolchain is the selected default host triple in the installer dialog. Depending on your system it should be either x86_64-pc-windows-msvc, i686-pc-windows-msvc, or aarch64-pc-windows-msvc.

If you already have Rust installed, you can make sure the correct toolchain is installed by running this command:
Terminal window

rustup default stable-msvc

Be sure to restart your Terminal (and in some cases your system) for the changes to take affect.

Next: Configure for Mobile Targets if you’d like to build for Android and iOS, or, if you’d like to use a JavaScript framework, install Node. Otherwise Create a Project.
Node.js

JavaScript ecosystem

Only if you intend to use a JavaScript frontend framework

    Go to the Node.js website, download the Long Term Support (LTS) version and install it.

    Check if Node was successfully installed by running:

Terminal window

node -v
# v20.10.0
npm -v
# 10.2.3

It’s important to restart your Terminal to ensure it recognizes the new installation. In some cases, you might need to restart your computer.

While npm is the default package manager for Node.js, you can also use others like pnpm or yarn. To enable these, run corepack enable in your Terminal. This step is optional and only needed if you prefer using a package manager other than npm.

Next: Configure for Mobile Targets or Create a project.
Configure for Mobile Targets

If you’d like to target your app for Android or iOS then there are a few additional dependencies that you need to install:

    Android
    iOS

Android

    Download and install Android Studio from the Android Developers website
    Set the JAVA_HOME environment variable:

    Linux
    macOS
    Windows

Terminal window

[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Android\Android Studio\jbr", "User")

    Use the SDK Manager in Android Studio to install the following:

    Android SDK Platform
    Android SDK Platform-Tools
    NDK (Side by side)
    Android SDK Build-Tools
    Android SDK Command-line Tools

Selecting “Show Package Details” in the SDK Manager enables the installation of older package versions. Only install older versions if necessary, as they may introduce compatibility issues or security risks.

    Set ANDROID_HOME and NDK_HOME environment variables.

    Linux
    macOS
    Windows

Terminal window

[System.Environment]::SetEnvironmentVariable("ANDROID_HOME", "$env:LocalAppData\Android\Sdk", "User")
$VERSION = Get-ChildItem -Name "$env:LocalAppData\Android\Sdk\ndk" | Select-Object -Last 1
[System.Environment]::SetEnvironmentVariable("NDK_HOME", "$env:LocalAppData\Android\Sdk\ndk\$VERSION", "User")

Tip

Most apps don’t refresh their environment variables automatically, so to let them pickup the changes, you can either restart your terminal and IDE or for your current PowerShell session, you can refresh it with
Terminal window

[System.Environment]::GetEnvironmentVariables("User").GetEnumerator() | % { Set-Item -Path "Env:\$($_.key)" -Value $_.value }

    Add the Android targets with rustup:

Terminal window

rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android

Next: Setup for iOS or Create a project.
iOS

macOS Only

iOS development requires Xcode and is only available on macOS. Be sure that you’ve installed Xcode and not Xcode Command Line Tools in the macOS system dependencies section.

    Add the iOS targets with rustup in Terminal:

Terminal window

rustup target add aarch64-apple-ios x86_64-apple-ios aarch64-apple-ios-sim

    Install Homebrew:

Terminal window

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    Install Cocoapods using Homebrew:

Terminal window

brew install cocoapods

Next: Create a project.
Troubleshooting

If you run into any issues during installation be sure to check the Troubleshooting Guide or reach out on the Tauri Discord.

Next Steps

Now that you’ve installed all of the prerequisites you’re ready to create your first Tauri project!



Create a Project

One thing that makes Tauri so flexible is its ability to work with virtually any frontend framework. We’ve created the create-tauri-app utility to help you create a new Tauri project using one of the officially maintained framework templates.

create-tauri-app currently includes templates for vanilla (HTML, CSS and JavaScript without a framework), Vue.js, Svelte, React, SolidJS, Angular, Preact, Yew, Leptos, and Sycamore. You can also find or add your own community templates and frameworks in the Awesome Tauri repo.

Alternatively, you can add Tauri to an existing project to quickly turn your existing codebase into a Tauri app.
Using create-tauri-app

To get started using create-tauri-app run one of the below commands in the folder you’d like to setup your project. If you’re not sure which command to use we recommend the Bash command on Linux and macOS and the PowerShell command on Windows.

    Bash
    PowerShell
    Fish
    npm
    Yarn
    pnpm
    deno
    bun
    Cargo

cargo install create-tauri-app --locked
cargo create-tauri-app

Follow along with the prompts to choose your project name, frontend language, package manager, and frontend framework, and frontend framework options if applicable.

Not sure what to choose?

We recommend starting with the vanilla template (HTML, CSS, and JavaScript without a frontend framework) to get started. You can always integrate a frontend framework later.

    Choose which language to use for your frontend: TypeScript / JavaScript
    Choose your package manager: pnpm
    Choose your UI template: Vanilla
    Choose your UI flavor: TypeScript

Scaffold a new project

    Choose a name and a bundle identifier (unique-id for your app):

    ? Project name (tauri-app) ›
    ? Identifier (com.tauri-app.app) ›

Select a flavor for your frontend. First the language:

? Choose which language to use for your frontend ›
Rust  (cargo)
TypeScript / JavaScript  (pnpm, yarn, npm, bun)
.NET  (dotnet)

Select a package manager (if there are multiple available):

Options for TypeScript / JavaScript:

? Choose your package manager ›
pnpm
yarn
npm
bun

Select a UI Template and flavor (if there are multiple available):

Options for Rust:

? Choose your UI template ›
Vanilla
Yew
Leptos
Sycamore

Options for TypeScript / JavaScript:

? Choose your UI template ›
Vanilla
Vue
Svelte
React
Solid
Angular
Preact

? Choose your UI flavor ›
TypeScript
JavaScript

Options for .NET:

? Choose your UI template ›
Blazor  (https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor/)

Once completed, the utility reports that the template has been created and displays how to run it using the configured package manager. If it detects missing dependencies on your system, it prints a list of packages and prompts how to install them.
Start the development server

After create-tauri-app has completed, you can navigate into your project’s folder, install dependencies, and then use the Tauri CLI to start the development server:

    npm
    yarn
    pnpm
    deno
    bun
    cargo

cd tauri-app
cargo tauri dev

You’ll now see a new window open with your app running.

Congratulations! You’ve made your Tauri app! 🚀
Manual Setup (Tauri CLI)

If you already have an existing frontend or prefer to set it up yourself, you can use the Tauri CLI to initialize the backend for your project separately.

Note

The following example assumes you are creating a new project. If you’ve already initialized the frontend of your application, you can skip the first step.

    Create a new directory for your project and initialize the frontend. You can use plain HTML, CSS, and JavaScript, or any framework you prefer such as Next.js, Nuxt, Svelte, Yew, or Leptos. You just need a way of serving the app in your browser. Just as an example, this is how you would setup a simple Vite app:
        npm
        yarn
        pnpm
        deno
        bun

    mkdir tauri-app
    cd tauri-app
    npm create vite@latest .

Then, install Tauri’s CLI tool using your package manager of choice. If you are using cargo to install the Tauri CLI, you will have to install it globally.

    npm
    yarn
    pnpm
    deno
    bun
    cargo

cargo install tauri-cli --version "^2.0.0" --locked

Determine the URL of your frontend development server. This is the URL that Tauri will use to load your content. For example, if you are using Vite, the default URL is http://localhost:5173.

In your project directory, initialize Tauri:

    npm
    yarn
    pnpm
    deno
    bun
    cargo

cargo tauri init

After running the command it will display a prompt asking you for different options:

✔ What is your app name? tauri-app
✔ What should the window title be? tauri-app
✔ Where are your web assets located? ..
✔ What is the url of your dev server? http://localhost:5173
✔ What is your frontend dev command? pnpm run dev
✔ What is your frontend build command? pnpm run build

This will create a src-tauri directory in your project with the necessary Tauri configuration files.

Verify your Tauri app is working by running the development server:

    npm
    yarn
    pnpm
    deno
    bun
    cargo

cargo tauri dev

    This command will compile the Rust code and open a window with your web content.

Congratulations! You’ve created a new Tauri project using the Tauri CLI! 🚀
Next Steps

    Add and Configure a Frontend Framework
    Tauri Command Line Interface (CLI) Reference
    Learn how to build your Tauri app
    Discover additional features to extend Tauri

Edit page


Upgrade from Tauri 2.0 Beta

This guide walks you through upgrading your Tauri 2.0 beta application to Tauri 2.0 release candidate.
Automated Migration

The Tauri v2 CLI includes a migrate command that automates most of the process and helps you finish the migration:

    npm
    yarn
    pnpm
    cargo

cargo install tauri-cli --version "^2.0.0" --locked
cargo tauri migrate

Learn more about the migrate command in the Command Line Interface reference
Breaking Changes

We have had several breaking changes going from beta to release candidate. These can be either auto-migrated (see above) or manually performed.
Tauri Core Plugins

We changed how Tauri built-in plugins are addressed in the capabilities PR #10390.

To migrate from the latest beta version you need to prepend all core permission identifiers in your capabilities with core: or switch to the core:default permission and remove old core plugin identifiers.

...
"permissions": [
    "path:default",
    "event:default",
    "window:default",
    "app:default",
    "image:default",
    "resources:default",
    "menu:default",
    "tray:default",
]
...

...
"permissions": [
    "core:path:default",
    "core:event:default",
    "core:window:default",
    "core:app:default",
    "core:image:default",
    "core:resources:default",
    "core:menu:default",
    "core:tray:default",
]
...

We also added a new special core:default permission set which will contain all default permissions of all core plugins, so you can simplify the permissions boilerplate in your capabilities config.

...
"permissions": [
    "core:default"
]
...

Built-In Development Server

We introduced changes to the network exposure of the built-in development server PR #10437 and PR #10456.

The built-in mobile development server no longer exposes network wide and tunnels traffic from the local machine directly to the device.

Currently this improvement does not automatically apply when running on iOS devices (either directly or from Xcode). In this case we default to using the public network address for the development server, but there’s a way around it which involves opening Xcode to automatically start a connection between your macOS machine and your connected iOS device, then running tauri ios dev --force-ip-prompt to select the iOS device’s TUN address (ends with ::2).

Your development server configuration needs to adapt to this change if running on a physical iOS device is intended. Previously we recommended checking if the TAURI_ENV_PLATFORM environment variable matches either android or ios, but since we can now connect to localhost unless using an iOS device, you should instead check the TAURI_DEV_HOST environment variable. Here’s an example of a Vite configuration migration:

    2.0.0-beta:

import { defineConfig } from 'vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';
import { internalIpV4Sync } from 'internal-ip';

const mobile = !!/android|ios/.exec(process.env.TAURI_ENV_PLATFORM);

export default defineConfig({
  plugins: [svelte()],
  clearScreen: false,
  server: {
    host: mobile ? '0.0.0.0' : false,
    port: 1420,
    strictPort: true,
    hmr: mobile
      ? {
          protocol: 'ws',
          host: internalIpV4Sync(),
          port: 1421,
        }
      : undefined,
  },
});

    2.0.0:

import { defineConfig } from 'vite';
import Unocss from 'unocss/vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';

const host = process.env.TAURI_DEV_HOST;

export default defineConfig({
  plugins: [svelte()],
  clearScreen: false,
  server: {
    host: host || false,
    port: 1420,
    strictPort: true,
    hmr: host
      ? {
          protocol: 'ws',
          host: host,
          port: 1430,
        }
      : undefined,
  },
});