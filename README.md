# BuckleScript FFI Annotations Cheat Sheet

This is a cheat sheet of the foreign function annotations available in BuckleScript, examples are in OCaml.

## Annotations for Bindings

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.val`|Bind to global value|`external dom : dom = "document" [@@bs.val]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_global_value_code_bs_val_code)|
`bs.scope "module-name"`|Bind to a value in a global module|`external random: unit -> float = "random" [@@bs.val][@@bs.scope "Math"]`|[BS-Docu](https://bucklescript.github.io/docs/en/bind-to-global-values.html#global-modules)|
`bs.new`|Bind to a Javascript constructor|`external create_date : unit -> t = "Date" [@@bs.new]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_javascript_constructor_code_bs_new_code)|
`bs.module "module-name"`|Bind to a value in a module|`external dirname: string -> string = "dirname" [@@bs.module "path"]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_a_value_from_a_module_code_bs_module_code)|
`bs.module "module-name" "NewName"`|To rename the module on import|`external dirname: string -> string = "dirname" [@@bs.module "path" "MyPath"]`||
`bs.send`|Bind to a method call on an object. This will create a function where the first parameter is the object.|`external getContext: canvas -> string -> context = "" [@@bs.send]` ||
`bs.send.pipe`|Same as `bs.send`, but uses the last parameter. Useful for chaining commands with `\|>`.|`external getContext: string -> canvas -> context = "" [@@bs.send.pipe]` |[Manual](https://bucklescript.github.io/docs/en/function.html#chaining)|
`bs.get_index`|Bind to dynamic array-style read access (`[]`) on a js object|external get : t -> int -> int = "" [@@bs.get_index]|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_dynamic_key_access_set_code_bs_set_index_code_code_bs_get_index_code)|
`bs.set_index`|Bind to dynamic array-style write access (`[]`) on a js object|external set : t -> int -> int -> unit = "" [@@bs.set_index]|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_dynamic_key_access_set_code_bs_set_index_code_code_bs_get_index_code)|
`bs.get`|Bind to get properties on js object|`external get_name : textarea -> string = "name" [@@bs.get]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_getter_setter_code_bs_get_code_code_bs_set_code)|
`bs.set`|Bind to get properties on js object|`external set_name : textarea -> string -> unit = "name" [@@bs.set]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_getter_setter_code_bs_get_code_code_bs_set_code)|
`bs.splice`|Bind to variadic functions by using arrays|`external join : string array -> string = "" [@@bs.module "path"] [@@bs.splice]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_splice_calling_convention_code_bs_splice_code)

## Creation of JS objects

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`%bs.obj`|Extension to create Js object directly|`let u = [%bs.obj { x = { y = { z = 3}}} ]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_create_js_objects_using_bs_obj)|
`bs.obj`|To create js objects with a function (with named parameters)|`external make_config : hi:int -> lo:int -> unit -> t = "" [@@bs.obj]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_create_js_objects_using_external)|

## Bindings to callbacks

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs`|Explicit uncurried callback|See manual for complete example|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#__code_bs_code_for_explicit_uncurried_callback)|
`bs.uncurry`|Implicit uncurried callback|See manual for complete example|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#__code_bs_uncurry_code_for_implicit_uncurried_callback_since_1_5_0)|
`bs.this`|Bind to `this` based callbacks|See manual for complete example|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_bindings_to_code_this_code_based_callbacks_code_bs_this_code)|


## Return type coercion

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.return`|Auto coercion for return values|`external getElementById : string -> element option = "" [@@bs.send.pipe:dom] [@@bs.return nullable]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_return_value_checking_since_1_5_1)|

## Mapping between OCaml and JS values

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.deriving`|Autogenerate mapping between OCaml and JS values |See manual for complete example|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_mapping_between_js_values_and_ocaml_values_since_2_1_0)|

## Embedding of untyped and raw JS code

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.external`|Detect the existence of a global variable|See manual for complete example|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_detect_global_variable_existence_code_bs_external_code_since_1_5_1)|
`%bs.raw`|Embed raw JS code (as string)|`let f = [%raw "function() {return 1}"]`|[BS-Docu](https://bucklescript.github.io/docs/en/embed-raw-javascript.html)|

## Special types on external declarations

(TODO)

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.string`||||
`bs.int`||||
`bs.ignore`||||
`bs.as`||||
`bs.unwrap`||||

## Regular Expressions

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`%bs.re`|Create JS regex|`let f = [%bs.re "/b/g"]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_regex_support)|

## Debugging & Node Macros

Annotation|When to use|Ocaml Example|Link| 
---|---|---|---|
`bs.debugger`|Embed the js debugger in a statement|`let f x y = [%bs.debugger]; x + y`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_debugger_support)
`bs.node __dirname`|Macro to access the Node `__dirname` variable|`let dirname : string option = [%bs.node __dirname]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_nodejs_special_variables_a_href_api_node_html_bs_node_a)|
`bs.node __filename`|Macro to access the Node `__filename` variable|`let filename : string option = [%bs.node __filename]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_nodejs_special_variables_a_href_api_node_html_bs_node_a)|
`bs.node _module`|Macro to access the Node `_module` variable|`let _module : Node.node_module option = [%bs.node _module]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_nodejs_special_variables_a_href_api_node_html_bs_node_a)|
`bs.node require`|Macro to access the Node `require` variable|`let require : Node.node_require option = [%bs.node require]`|[Manual](https://bucklescript.github.io/bucklescript/Manual.html#_binding_to_nodejs_special_variables_a_href_api_node_html_bs_node_a)|
