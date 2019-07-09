## Introduction

This repository is created for managing and deploying [Clova custom extension developer guide](https://pages.oss.navercorp.com/JTF-P6/Clova_Custom_Extension_Developer_Guide/) content. [GitBook](https://toolchain.gitbook.com/) is used for building document in PDF format and web format.

See the following sections for more details:

* [How to build documentation](#HowToBuild)
* [Branches](#Branches)
* [Releases](#Releases)

<a name="HowToBuild" />

## How to build documentation

The process of building documentation slightly differs depending on your operating system. Follow the steps required for building documentation on your operating system of choice below.

### Building documentation on macOS

Prepare the following tools and libraries:

* [`npm`](https://www.npmjs.com/get-npm)
* [`gitbook-cli`](https://toolchain.gitbook.com/setup.html)
* [`calibre`](https://toolchain.gitbook.com/ebook.html)

> Calibre is required for building PDF documentation. You must place the `ebook-convert` bin file in the $PATH contained in the Calibre application.

> You can also make documentation build environment with Docker. See [Setup GitBook env. with Docker](https://oss.navercorp.com/JTF-P6/platform_technical_doc/wiki/Setup-GitBook-env-with-Docker)

To build documentation, follow the next steps:

1. Clone this repository.

```bash
git clone https://oss.navercorp.com/JTF-P6/Clova_Custom_Extension_Developer_Guide.git
```

2. (Required once) Install required gitbook plugins.

```bash
gitbook install
```

3. Run one of the following commands:

```bash
# If you want to build a PDF file.
gitbook pdf . book.pdf

# If you want to build web content in ./book directory
gitbook build . book

# If you want to run a web server with GitBook web content. You can access the web server at http://localhost:4000.
gitbook serve
```

### Building documentation on Windows

Prepare the following tools and libraries:

* [`npm`](https://www.npmjs.com/get-npm)
* [`gitbook-cli`](https://toolchain.gitbook.com/setup.html)
* [`calibre`](https://toolchain.gitbook.com/ebook.html)

> Calibre is required for building PDF documentation. You must place the `ebook-convert` bin file in the $PATH contained in the Calibre application.

To build documentation, follow the next steps:

1. Clone this repository.

```bash
git clone https://oss.navercorp.com/JTF-P6/Clova_Custom_Extension_Developer_Guide.git
```

2. (Required once) Install required plugins.

```bash
# Install gitbook-cli
npm install gitbook-cli -g

# Install ebook-convert (Calibre must be already installed)
npm install ebook-convert -g

# Install svgexport
npm install svgexport -g
```

3. (Required once) Install Gitbook in your cloned repository.

```bash
gitbook install
```

4. Run one of the following commands:

```bash
# If you want to build a PDF file.
gitbook pdf . book.pdf

# If you want to build web content in ./book directory
gitbook build . book

# If you want to run a web server with GitBook web content. You can access the web server at http://localhost:4000.
gitbook serve
```

<a name="Branches" />

## Branches

Due to the various purposes, this repository has the following branches:

<ul>
  <li>Common branches
    <ul>
      <li><strong>document</strong><br />Main branch that holds whole content of Clova custom extension developer guide. Working branches are forked from this branch and after job is done the branches are merged back to this branch.</li>
      <li><strong>master</strong> (document -> master)<br />Branch for internal documentation release</li>
    </ul>
  </li>
  <li>Branches for Japan business
    <ul>
      <li><strong>doc-JP-Xxxx</strong> (document -> doc-JP-Xxxx)<br />Branch for speficif business or partners in Japan</li>
      <li><strong>doc-JP-Public</strong> (document -> doc-JP-Public)<br />Branch for public documentation which is deployed on Clova devloper center (JP) that interacts with LINE developer site.</li>
    </ul>
  </li>
  <li>Branches for Korean business
    <ul>
      <li><strong>doc-KR-Xxxx</strong> (document -> doc-KR-Xxxx)<br />Branch for Korean business, this branch holds KO and EN version documentation.</li>
      <li><strong>doc-KR-Public</strong> (document -> doc-KR-Public)<br />Branch for public documentation which is deployed on NAVER developer center.</li>
    </ul>
  </li>
</ul>

<a name="Releases" />

## Releases

The release history of Clova custom extension developer guide is recorded as GitHub issuse and release feature as below:

* [Issues with Deployment label](https://oss.navercorp.com/JTF-P6/Clova_Custom_Extension_Developer_Guide/issues?utf8=%E2%9C%93&q=is%3Aissue%20label%3ADeployment%20)
* [Releases](https://oss.navercorp.com/JTF-P6/Clova_Custom_Extension_Developer_Guide/releases)

> Due to the translation job, EN version documentation is released serveral weeks later after KO version release.
