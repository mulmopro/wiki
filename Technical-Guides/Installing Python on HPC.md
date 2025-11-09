There may be a use case for installing Python on a HPC cluster. This is a guide for how to do that.
With "HPC" we also mean in general a remote machine on which you do not have root access, as it is the case with the HPC@POLITO cluster, or the italian supercomputers in CINECA.

## Why and how

You may need to run code with a specific version of Python, or with specific packages installed.

One example is if you want to run code for interpreting yaml files: the package `PyYAMLs` or `ruamel` are only working with Python 3.5 or higher.

So: how to install Python on HPC, and how to maintain it by installing packages with `pip`?

We will start by managing the latter.
If we were to install Python from source, then we wouldn't have SSL support, which is necessary for `pip` to securely download packages.
To resolve this issue, you will need to install the necessary SSL development libraries and build Python with SSL support (or re-build if you already have Python installed).

## **Install the OpenSSL development libraries:**

You need to install the OpenSSL development libraries in your local directory, as you don't have admin privileges. First, download and extract the OpenSSL source code:

Go to a directory where you want to install OpenSSL (any will do), and download the source code. In this guide, we will use `your_directory` as the installation path, both for OpenSSL and Python.

```bash
wget https://www.openssl.org/source/openssl-1.1.1l.tar.gz
tar -xzf openssl-1.1.1l.tar.gz
```
Now, navigate to the extracted directory, configure, compile, and install OpenSSL:

```bash
cd openssl-1.1.1l
./config --prefix=your_directory/openssl no-shared
make
make install
```

Replace `your_directory` with the desired installation path.

#### **Clean previous Python installations**
If you had a previous local Python installation you compiled, you will need to clean it before building Python again with SSL support. If you don't have a previous Python installation, you can skip this step and proceed to the next chapter, "**Install Python from source**".

To clean a previous build, navigate to the Python source code directory and run the clean command:

```bash
cd your_directory/Python-3.9.10
make clean
```


## **Install Python from source:**


1. **Download the Python source code:**
First, visit the official Python downloads page (https://www.python.org/downloads/) and find the version of Python you want to install. Right-click on the "Source tarball" link and copy the link address.

Next, log in to your HPC cluster and download the source code using `wget` or `curl`. For example, to download Python 3.9.10, you can use the following command:

```bash
wget https://www.python.org/ftp/python/3.9.10/Python-3.9.10.tgz
```

2. **Extract the source code:**
Extract the downloaded tarball using the following command:

```bash
tar -xzf Python-3.9.10.tgz
```

3. **Configure and install Python:**
Navigate to the extracted source code directory and run the `configure` script. Use the `--prefix` option to specify a directory where you want to install Python locally (e.g., in your home directory).
Also, you're going to configure Python with the correct paths to the locally installed OpenSSL libraries.

Remember to replace `your_directory` with the actual installation path.

```bash
cd Python-3.9.10
./configure --prefix=your_directory/python-3.9.10 \
            --with-openssl=your_directory/openssl
```

After the configuration is complete, compile and install Python:

```bash
make
make install
```

4. **Update your PATH:**
To use the newly installed Python version, you need to update your `PATH` environment variable. Add the following lines to your shell's configuration file (e.g., `~/.bashrc` for `bash`, `~/.zshrc` for `zsh`), replacing `your_directory` with the actual installation path:

```bash
export PATH=your_directory/python-3.9.10/bin:$PATH
```

Save the file and run `source ~/.bashrc` (or the appropriate command for your shell) to apply the changes.

5. **Verify the new Python installation:**
Check that the new Python version is being used by running the following command. Check that `your_directory/python-3.9.10/bin` folder contains a `python3` executable, otherwise change it (for example it could be `python3.9`)

```bash
python3 --version
```

## **Use your newly compiled Python installation**

Now you should be able to use your new, local, Python installation.
To test it out, you can create a virtual environment using a virtual environment, which you can learn about in the [[Python Virtual Environments]] page in this wiki.

A good resource on the web is also: [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/) article.







