# classlib-template

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Actions Status](https://github.com/DDTH/classlib-template/workflows/ci/badge.svg)](https://github.com/DDTH/classlib-template/actions)
[![Release](https://img.shields.io/github/release/DDTH/classlib-template.svg?style=flat-square)](RELEASE-NOTES.md)

Template project to quickly spin up a C# Class Library project.

## Features

- Sample solution with a class library project and a matching xUnit test project.
- The class library is ready to be packaged and published as a NuGet package.
- Placeholders for README, LICENSE (MIT), and RELEASE-NOTES.
- Pre-configured `.gitignore` and `.editorconfig` files.
- Ready-to-use GitHub Actions workflow for building, testing and releasing the library to NuGet.

## Usage

Install (or update) the package from NuGet to make the template available:

```sh
$ dotnet new install Ddth.Templates.Classlib
```

After the package is installed, create a new solution using the template:

```sh
$ dotnet new clt -n MyLib
```

The above command will create a new solution named `MyLib` in the current directory.

**Happy coding!**

After creating your solution, remember to:
- [ ] Update the package metadata (`PackageId`, `Authors`, `Description`, etc.) in the project file.
- [ ] Fill in `README.md`, `LICENSE.md`, and `RELEASE-NOTES.md`.
- [ ] Configure the GitHub Actions workflow secrets (e.g. the NuGet API key) for releasing.

🌟 If you find this project useful, please star it. 🌟

## License

This template is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Contributing & Support

Feel free to create [pull requests](https://github.com/DDTH/classlib-template/compare/contrib_wait_to_merge...) or [issues](https://github.com/DDTH/classlib-template/issues) to report bugs or suggest new features.

> Please create PRs against the `contrib_wait_to_merge` branch.
