#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM debian:jessie-slim

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# runtime dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
		ca-certificates \
		netbase \
	&& rm -rf /var/lib/apt/lists/*

ENV GPG_KEY 0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D
ENV PYTHON_VERSION 3.6.6

RUN set -ex \
	\
	&& savedAptMark="$(apt-mark showmanual)" \
	&& apt-get update && apt-get install -y --no-install-recommends \
		dpkg-dev \
		gcc \
		libbz2-dev \
		libc6-dev \
		libexpat1-dev \
		libffi-dev \
		libgdbm-dev \
		liblzma-dev \
		libncursesw5-dev \
		libreadline-dev \
		libsqlite3-dev \
		libssl-dev \
		make \
		tk-dev \
		wget \
		xz-utils \
		zlib1g-dev \
# as of Stretch, "gpg" is no longer included by default
		$(command -v gpg > /dev/null || echo 'gnupg dirmngr') \
	\
	&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& { command -v gpgconf > /dev/null && gpgconf --kill all || :; } \
	&& rm -rf "$GNUPGHOME" python.tar.xz.asc \
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-loadable-sqlite-extensions \
		--enable-shared \
		--with-system-expat \
		--with-system-ffi \
		--without-ensurepip \
	&& make -j "$(nproc)" \
	&& make install \
	&& ldconfig \
	\
	&& apt-mark auto '.*' > /dev/null \
	&& apt-mark manual $savedAptMark \
	&& find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' \
		| awk '/=>/ { print $(NF-1) }' \
		| sort -u \
		| xargs -r dpkg-query --search \
		| cut -d: -f1 \
		| sort -u \
		| xargs -r apt-mark manual \
	&& apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
	&& rm -rf /var/lib/apt/lists/* \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' + \
	&& rm -rf /usr/src/python \
	\
	&& python3 --version

# make some useful symlinks that are expected to exist
RUN cd /usr/local/bin \
	&& ln -s idle3 idle \
	&& ln -s pydoc3 pydoc \
	&& ln -s python3 python \
	&& ln -s python3-config python-config

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 18.1

RUN set -ex; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get update; \
	apt-get install -y --no-install-recommends wget; \
	\
	wget -O get-pip.py 'https://bootstrap.pypa.io/get-pip.py'; \
	\
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*; \
	\
	python get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		"pip==$PYTHON_PIP_VERSION" \
	; \
	pip --version; \
	\
	find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' +; \
	rm -f get-pip.py
	
# HERE are Andrey's changes are comming.

RUN apt-get -y update 
RUN apt-get install -y apt-utils
RUN apt-get install -y git-core
RUN yes | pip install gunicorn
# Installing geo-tools
RUN apt install -y gcc
RUN apt install -y build-essential
RUN apt-get install -y gdal-bin
RUN apt-get install -y screen
RUN yes | pip install alabaster
RUN yes | pip install anaconda-client
RUN yes | pip install anaconda-project
RUN yes | pip install aniso8601
RUN yes | pip install argparse
RUN yes | pip install arrow
RUN yes | pip install asn1crypto
RUN yes | pip install astroid
RUN yes | pip install astropy
RUN yes | pip install atomicwrites
RUN yes | pip install attrs
RUN yes | pip install backcall
RUN yes | pip install backports.shutil-get-terminal-size
RUN yes | pip install bcrypt
RUN yes | pip install beautifulsoup4
RUN yes | pip install bitarray
RUN yes | pip install bkcharts
RUN yes | pip install blaze
RUN yes | pip install bleach
RUN yes | pip install blinker
RUN yes | pip install bokeh
RUN yes | pip install boto
RUN yes | pip install boto3
RUN yes | pip install botocore
RUN yes | pip install branca
RUN yes | pip install cachetools
RUN yes | pip install certifi
RUN yes | pip install cffi
RUN yes | pip install chardet
RUN yes | pip install click
RUN yes | pip install cloudpickle
RUN yes | pip install clyent
RUN yes | pip install colorama
RUN yes | pip install comtypes
RUN yes | pip install contextlib2
RUN yes | pip install cryptography
RUN yes | pip install cycler
RUN yes | pip install cytoolz
RUN yes | pip install dask
RUN yes | pip install datashape
RUN yes | pip install decorator
RUN yes | pip install dill
RUN yes | pip install distlib
RUN yes | pip install distributed
RUN yes | pip install docutils
RUN yes | pip install dominate
RUN yes | pip install ecdsa
RUN yes | pip install entrypoints
RUN yes | pip install et-xmlfile
RUN yes | pip install eventbrite
RUN yes | pip install fastcache
RUN yes | pip install filelock
RUN yes | pip install flask-marshmallow
RUN yes | pip install folium
RUN yes | pip install future
RUN yes | pip install gapic-google-cloud-datastore-v1
RUN yes | pip install gapic-google-cloud-error-reporting-v1beta1
RUN yes | pip install gapic-google-cloud-logging-v2
RUN yes | pip install geocoder
RUN yes | pip install geographiclib
RUN yes | pip install geojson
RUN yes | pip install geopy
RUN yes | pip install gevent
RUN yes | pip install glob2
RUN yes | pip install google
RUN yes | pip install google-api-core
RUN yes | pip install google-auth
RUN yes | pip install google-cloud
RUN yes | pip install google-cloud-bigquery
RUN yes | pip install google-cloud-bigquery-datatransfer
RUN yes | pip install google-cloud-bigtable
RUN yes | pip install google-cloud-container
RUN yes | pip install google-cloud-core
RUN yes | pip install google-cloud-datastore
RUN yes | pip install google-cloud-dns
RUN yes | pip install google-cloud-error-reporting
RUN yes | pip install google-cloud-firestore
RUN yes | pip install google-cloud-language
RUN yes | pip install google-cloud-logging
RUN yes | pip install google-cloud-monitoring
RUN yes | pip install google-cloud-pubsub
RUN yes | pip install google-cloud-resource-manager
RUN yes | pip install google-cloud-runtimeconfig
RUN yes | pip install google-cloud-spanner
RUN yes | pip install google-cloud-speech
RUN yes | pip install google-cloud-storage
RUN yes | pip install google-cloud-trace
RUN yes | pip install google-cloud-translate
RUN yes | pip install google-cloud-videointelligence
RUN yes | pip install google-cloud-vision
RUN yes | pip install google-gax
RUN yes | pip install google-resumable-media
RUN yes | pip install googleapis-common-protos
RUN yes | pip install googlemaps
RUN yes | pip install greenlet
RUN yes | pip install grpc-google-iam-v1
RUN yes | pip install grpcio
RUN yes | pip install h5py
RUN yes | pip install holidays
RUN yes | pip install html5lib
RUN yes | pip install httplib2
RUN yes | pip install idna
RUN yes | pip install image
RUN yes | pip install imageio
RUN yes | pip install imagesize
RUN yes | pip install ipykernel
RUN yes | pip install ipython
RUN yes | pip install ipython-genutils
RUN yes | pip install ipywidgets
RUN yes | pip install isort
RUN yes | pip install itsdangerous
RUN yes | pip install jdcal
RUN yes | pip install jedi
RUN yes | pip install jmespath
RUN yes | pip install jsonschema
RUN yes | pip install jupyter
RUN yes | pip install jupyter-client
RUN yes | pip install jupyter-console
RUN yes | pip install jupyter-core
RUN yes | pip install jupyterlab
RUN yes | pip install jupyterlab-launcher
RUN yes | pip install jwt
RUN yes | pip install kiwisolver
RUN yes | pip install lazy-object-proxy
RUN yes | pip install llvmlite
RUN yes | pip install locket
RUN yes | pip install lockfile
RUN yes | pip install lxml
RUN yes | pip install marshmallow
RUN yes | pip install marshmallow-mongoengine
RUN yes | pip install matplotlib
RUN yes | pip install mccabe
RUN yes | pip install mistune
RUN yes | pip install mongoengine
RUN yes | pip install more-itertools
RUN yes | pip install mpmath
RUN yes | pip install msgpack
RUN yes | pip install msgpack-python
RUN yes | pip install multipledispatch
RUN yes | pip install nbconvert
RUN yes | pip install nbformat
RUN yes | pip install networkx
RUN yes | pip install nltk
RUN yes | pip install nose
RUN yes | pip install notebook
RUN yes | pip install numba
RUN yes | pip install numexpr
RUN yes | pip install numpy
RUN yes | pip install numpydoc
RUN yes | pip install oauth2client
RUN yes | pip install oauthlib
RUN yes | pip install odo
RUN yes | pip install olefile
RUN yes | pip install openpyxl
RUN yes | pip install osmread
RUN yes | pip install ouimeaux
RUN yes | pip install overpass
RUN yes | pip install overpy
RUN yes | pip install packaging
RUN yes | pip install pandas
RUN yes | pip install pandocfilters
RUN yes | pip install parso
RUN yes | pip install partd
RUN yes | pip install path.py
RUN yes | pip install pathlib2
RUN yes | pip install patsy
RUN yes | pip install pep8
RUN yes | pip install pickleshare
RUN yes | pip install pip
RUN yes | pip install pkginfo
RUN yes | pip install pluggy
RUN yes | pip install ply
RUN yes | pip install progress
RUN yes | pip install prompt-toolkit
RUN yes | pip install proto-google-cloud-datastore-v1
RUN yes | pip install proto-google-cloud-error-reporting-v1beta1
RUN yes | pip install proto-google-cloud-logging-v2
RUN yes | pip install protobuf
RUN yes | pip install psutil
RUN yes | pip install py
RUN yes | pip install pyOpenSSL
RUN yes | pip install pyasn1
RUN yes | pip install pyasn1-modules
RUN yes | pip install pycodestyle
RUN yes | pip install pycosat
RUN yes | pip install pycparser
RUN yes | pip install pycurl
RUN yes | pip install pyflakes
RUN yes | pip install pylint
RUN yes | pip install pymongo
RUN yes | pip install pyodbc
RUN yes | pip install pyparsing
RUN yes | pip install pyproj
RUN yes | pip install pyreadline
RUN yes | pip install pytest
RUN yes | pip install python-dateutil
RUN yes | pip install python-jose
RUN yes | pip install pytz
RUN yes | pip install pywin32
RUN yes | pip install pywinpty
RUN yes | pip install pyzmq
RUN yes | pip install qtconsole
RUN yes | pip install queuelib
RUN yes | pip install r360-py
RUN yes | pip install ratelim
RUN yes | pip install requests
RUN yes | pip install requests-oauthlib
RUN yes | pip install rope
RUN yes | pip install rsa
RUN yes | pip install ruamel.yaml
RUN yes | pip install s3transfer
RUN yes | pip install scikit-image
RUN yes | pip install scikit-learn
RUN yes | pip install scipy
RUN yes | pip install seaborn
RUN yes | pip install setuptools
RUN yes | pip install simplegeneric
RUN yes | pip install singledispatch
RUN yes | pip install sip
RUN yes | pip install six
RUN yes | pip install sklearn
RUN yes | pip install slackclient
RUN yes | pip install snowballstemmer
RUN yes | pip install sortedcollections
RUN yes | pip install sortedcontainers
RUN yes | pip install sphinxcontrib-websupport
RUN yes | pip install spyder
RUN yes | pip install statsmodels
RUN yes | pip install sympy
RUN yes | pip install tables
RUN yes | pip install tblib
RUN yes | pip install termcolor
RUN yes | pip install terminado
RUN yes | pip install testpath
RUN yes | pip install toolz
RUN yes | pip install tornado
RUN yes | pip install traitlets
RUN yes | pip install typing
RUN yes | pip install unicodecsv
RUN yes | pip install urllib3
RUN yes | pip install visitor
RUN yes | pip install wcwidth
RUN yes | pip install webencodings
RUN yes | pip install websocket-client
RUN yes | pip install wheel
RUN yes | pip install widgetsnbextension
RUN yes | pip install win-inet-pton
RUN yes | pip install win-unicode-console
RUN yes | pip install wincertstore
RUN yes | pip install wrapt
RUN yes | pip install xlrd
RUN yes | pip install xlwings
RUN yes | pip install xlwt
RUN yes | pip install xmltodict
RUN yes | pip install zict
RUN yes | pip install zope.interface




CMD ["python3"]
