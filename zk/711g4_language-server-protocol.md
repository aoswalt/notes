# Language Server Protocol

#lsp #api

The [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) is an [api specification](https://microsoft.github.io/language-server-protocol/specifications/specification-current/) to decouple editor features from the language-specific implementation. This separation allows the language-specific server to intimately understand the language, often being developed in the language itself, and the editor to stay focused on its features without having to understand each language to support it.

For example, given 2 editors, Vim and VS Code, and 2 languages, Elixir and JavaScript.

Without lsp:

* vim implements Elixir features
* vim implements JavaScript features
* code implements Elixir features
* code implements JavaScript features

With lsp:

* vim implements an lsp client
* code implements an lsp client
* Elixir provides an lsp server
* JavaScript provides an lsp server

Now, 1 new editor, Emacs, and 1 new language, Rust are added

Without lsp:

* Vim implements Rust features
* Code implements Rust features
* Emacs implements Elixir features
* Emacs implements JavaScript features
* Emacs implements Rust features

With lsp:

* Emacs implements an lsp client
* Rust provides an lsp server

No work must be done to existing editors when leveraging the language server protocol because of the common interface.
