# install-R
A way to install a local instance of R when you don't have root access.


> ### Note
These instructions are for Linux and more specifically for "Ubuntu 12.04.5 LTS". It may work on other versions, so play with | it

## Install some directories
Setup some initial directories
```
$ mkdir $HOME/packages   # base directory for your custom installation
$ mkdir $HOME/packages/rpkgs  # local R library directory
$ mkdir $HOME/packages/tmp  # tmp directory, if you need it
```


## Install the latest [zlib](http://zlib.net/)
Ubuntu 12.04 comes with zlib v1.2.3.4. But the minimum requirement for R-3.3.2 is >1.2.5. Let's go ahead and install a local version
```
$ cd $HOME/packages
$ wget http://zlib.net/zlib-1.2.8.tar.gz
$ tar xzvf zlib-1.2.8.tar.gz
$ cd zlib-1.2.8
$ ./configure --prefix=$HOME/packages
$ make
$ make install
```


## Install the latest [cURL](https://curl.haxx.se/)
Ubuntu 12.04 comes with cURL v7.22.0. But the minimum requirement for R-3.3.2 is >7.28.0. Let's go ahead and install a local version
```
$ cd $HOME/packages
$ wget -v https://curl.haxx.se/download/curl-7.51.0.tar.gz
$ tar -xvzf curl-7.51.0.tar.gz 
$ cd curl-7.51.0
$ ./configure --prefix=$HOME/packages
$ make -j3
$ make install
```


## Setup environmental variables
```
$ export TMPDIR=$HOME/packages/tmp
$ export CONFIGURE_OPTS="--prefix=$HOME/R332 --enable-R-shlib --with-cairo"
$ export PATH=$HOME/packages/bin:$PATH
$ export LD_LIBRARY_PATH=$HOME/packages/lib:$LD_LIBRARY_PATH 
$ export CFLAGS="-I$HOME/packages/include" 
$ export LDFLAGS="-L$HOME/packages/lib" 
```



## Install [Renv](https://github.com/viking/Renv) (instructions copied here)

```
$ cd
$ git clone git://github.com/viking/Renv.git .Renv
$ export PATH=$PATH:$HOME/.Renv/bin
$ eval "$(Renv init -)"
$ exec $SHELL
```


## Install [R-build](https://github.com/viking/R-build) to make things a bit more easier
```
$ mkdir -p ~/.Renv/plugins
$ cd ~/.Renv/plugins
$ git clone git://github.com/viking/R-build.git
```


## Test Renv 
```
$ Renv
```


> ### Note
The above command should return a usage response. If it doesn't or throws an error, you may need to re-run `$(Renv init -)` to fully initialize Renv


## Install a specific version of R
```
$ Renv install 3.3.2
```

This will install `R` in `.Renv/versions`


## Running local version of R
```
$ Renv shell 3.3.2
$ R RHOME # displays which R is being used
```


## Unsetting local R and using system-wide R
```
$ Renv shell --unset
$ R RHOME # displays which R is being used
```


## Install and load a R package, e.g. [ctmm](https://cran.r-project.org/package=ctmm)
```
$ Renv shell 3.3.2
$ R
> install.packages("ctmm", lib="/home/ajay/packages/rpkgs")
> library("ctmm", lib.loc="/home/ajay/packages/rpkgs/")
```


## Done.
Pop open a beer.
