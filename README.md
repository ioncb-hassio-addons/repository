# IonCB Home Assistant Apps

![Project Stage][project-stage-shield]
![Maintenance][maintenance-shield]
[![License][license-shield]](LICENSE.md)

## About

Home Assistant allows anyone to create app repositories to share their
apps for Home Assistant easily. This repository is one of those repositories,
providing extra Home Assistant apps for your installation.

The primary goal of this project is to provide you (as a Home Assistant user)
with additional, high quality, apps that allow you to take your automated
home to the next level.

## Installation

In general, there is no need to install this repository on your
Home Assistant instance. It is activated and added by Home Assistant
by default.

However, if the repository is missing on your setup, adding this apps
repository to your Home Assistant instance is pretty easy. In the
Home Assistant app store, a possibility to add a repository is provided.

Use the following URL to add this repository:

```txt
https://github.com/ioncb-hassio-addons/repository
```

## Apps provided by this repository

### &#10003; [Example][addon-example]

![Latest Version][example-version-shield]
![Supports armhf Architecture][example-armhf-shield]
![Supports armv7 Architecture][example-armv7-shield]
![Supports aarch64 Architecture][example-aarch64-shield]
![Supports amd64 Architecture][example-amd64-shield]
![Supports i386 Architecture][example-i386-shield]

Example app by Home Assistant Community Apps

[:books: Example app documentation][addon-doc-example]

## Releases

Releases are based on [Semantic Versioning][semver], and use the format
of ``MAJOR.MINOR.PATCH``. In a nutshell, the version will be incremented
based on the following:

- ``MAJOR``: Incompatible or major changes.
- ``MINOR``: Backwards-compatible new features and enhancements.
- ``PATCH``: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You could also open an issue here on GitHub. Note, we use a separate
GitHub repository for each app. Please ensure you are creating the issue
on the correct GitHub repository matching the app.

- [Open an issue for the app: Example][example-issue]

For a general repository issue or app ideas [open an issue here][issue]

## Adding a new app

We are currently not accepting third party apps to this repository.

For questions, please contact [Jhon Coronel][jhoncb]:

- Drop him an email: jcoronel@ioncb.com

## License

MIT License

Copyright (c) 2026 Jhon Coronel

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[addon-example]: https://github.com/ioncb-hassio-addons/app-example/tree/v0.0.11
[addon-doc-example]: https://github.com/ioncb-hassio-addons/app-example/blob/v0.0.11/README.md
[example-issue]: https://github.com/ioncb-hassio-addons/app-example/issues
[example-version-shield]: https://img.shields.io/badge/version-v0.0.11-blue.svg
[example-aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[example-amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[example-armhf-shield]: https://img.shields.io/badge/armhf-no-red.svg
[example-armv7-shield]: https://img.shields.io/badge/armv7-no-red.svg
[example-i386-shield]: https://img.shields.io/badge/i386-no-red.svg
[awesome-shield]: https://img.shields.io/badge/awesome%3F-yes-brightgreen.svg
[awesome]: https://awesome-ha.com
[jhoncb]: https://github.com/jhoncb
[issue]: https://github.com/ioncb-hassio-addons/repository/issues
[license-shield]: https://img.shields.io/github/license/ioncb-hassio-addons/repository.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2026.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-production%20ready-brightgreen.svg
[semver]: http://semver.org/spec/v2.0.0.html