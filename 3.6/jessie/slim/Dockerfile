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
RUN apt-get update 
RUN apt-get install git-core
RUN pip install gunicorn
# Installing geo-tools
RUN apt install gcc
RUN apt install build-essential
RUN apt-get install gdal-bin
RUN apt-get install screen


RUN pip install alabaster
RUN pip install anaconda-client
RUN pip install anaconda-project
RUN pip install aniso8601
RUN pip install argparse
RUN pip install arrow
RUN pip install asn1crypto
RUN pip install astroid
RUN pip install astropy
RUN pip install atomicwrites
RUN pip install attrs
RUN pip install backcall
RUN pip install backports.shutil-get-terminal-size
RUN pip install bcrypt
RUN pip install beautifulsoup4
RUN pip install bitarray
RUN pip install bkcharts
RUN pip install blaze
RUN pip install bleach
RUN pip install blinker
RUN pip install bokeh
RUN pip install boto
RUN pip install boto3
RUN pip install botocore
RUN pip install branca
RUN pip install cachetools
RUN pip install certifi
RUN pip install cffi
RUN pip install chardet
RUN pip install click
RUN pip install cloudpickle
RUN pip install clyent
RUN pip install colorama
RUN pip install comtypes
RUN pip install contextlib2
RUN pip install cryptography
RUN pip install cycler
RUN pip install cytoolz
RUN pip install dask
RUN pip install datashape
RUN pip install decorator
RUN pip install dill
RUN pip install distlib
RUN pip install distributed
RUN pip install docutils
RUN pip install dominate
RUN pip install ecdsa
RUN pip install entrypoints
RUN pip install et-xmlfile
RUN pip install eventbrite
RUN pip install fastcache
RUN pip install filelock
RUN pip install flask-marshmallow
RUN pip install folium
RUN pip install future
RUN pip install gapic-google-cloud-datastore-v1
RUN pip install gapic-google-cloud-error-reporting-v1beta1
RUN pip install gapic-google-cloud-logging-v2
RUN pip install geocoder
RUN pip install geographiclib
RUN pip install geojson
RUN pip install geopy
RUN pip install gevent
RUN pip install glob2
RUN pip install google
RUN pip install google-api-core
RUN pip install google-auth
RUN pip install google-cloud
RUN pip install google-cloud-bigquery
RUN pip install google-cloud-bigquery-datatransfer
RUN pip install google-cloud-bigtable
RUN pip install google-cloud-container
RUN pip install google-cloud-core
RUN pip install google-cloud-datastore
RUN pip install google-cloud-dns
RUN pip install google-cloud-error-reporting
RUN pip install google-cloud-firestore
RUN pip install google-cloud-language
RUN pip install google-cloud-logging
RUN pip install google-cloud-monitoring
RUN pip install google-cloud-pubsub
RUN pip install google-cloud-resource-manager
RUN pip install google-cloud-runtimeconfig
RUN pip install google-cloud-spanner
RUN pip install google-cloud-speech
RUN pip install google-cloud-storage
RUN pip install google-cloud-trace
RUN pip install google-cloud-translate
RUN pip install google-cloud-videointelligence
RUN pip install google-cloud-vision
RUN pip install google-gax
RUN pip install google-resumable-media
RUN pip install googleapis-common-protos
RUN pip install googlemaps
RUN pip install greenlet
RUN pip install grpc-google-iam-v1
RUN pip install grpcio
RUN pip install h5py
RUN pip install holidays
RUN pip install html5lib
RUN pip install httplib2
RUN pip install idna
RUN pip install image
RUN pip install imageio
RUN pip install imagesize
RUN pip install ipykernel
RUN pip install ipython
RUN pip install ipython-genutils
RUN pip install ipywidgets
RUN pip install isort
RUN pip install itsdangerous
RUN pip install jdcal
RUN pip install jedi
RUN pip install jmespath
RUN pip install jsonschema
RUN pip install jupyter
RUN pip install jupyter-client
RUN pip install jupyter-console
RUN pip install jupyter-core
RUN pip install jupyterlab
RUN pip install jupyterlab-launcher
RUN pip install jwt
RUN pip install kiwisolver
RUN pip install lazy-object-proxy
RUN pip install llvmlite
RUN pip install locket
RUN pip install lockfile
RUN pip install lxml
RUN pip install marshmallow
RUN pip install marshmallow-mongoengine
RUN pip install matplotlib
RUN pip install mccabe
RUN pip install mistune
RUN pip install mongoengine
RUN pip install more-itertools
RUN pip install mpmath
RUN pip install msgpack
RUN pip install msgpack-python
RUN pip install multipledispatch
RUN pip install nbconvert
RUN pip install nbformat
RUN pip install networkx
RUN pip install nltk
RUN pip install nose
RUN pip install notebook
RUN pip install numba
RUN pip install numexpr
RUN pip install numpy
RUN pip install numpydoc
RUN pip install oauth2client
RUN pip install oauthlib
RUN pip install odo
RUN pip install olefile
RUN pip install openpyxl
RUN pip install osmread
RUN pip install ouimeaux
RUN pip install overpass
RUN pip install overpy
RUN pip install packaging
RUN pip install pandas
RUN pip install pandocfilters
RUN pip install parso
RUN pip install partd
RUN pip install path.py
RUN pip install pathlib2
RUN pip install patsy
RUN pip install pep8
RUN pip install pickleshare
RUN pip install pip
RUN pip install pkginfo
RUN pip install pluggy
RUN pip install ply
RUN pip install progress
RUN pip install prompt-toolkit
RUN pip install proto-google-cloud-datastore-v1
RUN pip install proto-google-cloud-error-reporting-v1beta1
RUN pip install proto-google-cloud-logging-v2
RUN pip install protobuf
RUN pip install psutil
RUN pip install py
RUN pip install pyOpenSSL
RUN pip install pyasn1
RUN pip install pyasn1-modules
RUN pip install pycodestyle
RUN pip install pycosat
RUN pip install pycparser
RUN pip install pycurl
RUN pip install pyflakes
RUN pip install pylint
RUN pip install pymongo
RUN pip install pyodbc
RUN pip install pyparsing
RUN pip install pyproj
RUN pip install pyreadline
RUN pip install pytest
RUN pip install python-dateutil
RUN pip install python-jose
RUN pip install pytz
RUN pip install pywin32
RUN pip install pywinpty
RUN pip install pyzmq
RUN pip install qtconsole
RUN pip install queuelib
RUN pip install r360-py
RUN pip install ratelim
RUN pip install requests
RUN pip install requests-oauthlib
RUN pip install rope
RUN pip install rsa
RUN pip install ruamel.yaml
RUN pip install s3transfer
RUN pip install scikit-image
RUN pip install scikit-learn
RUN pip install scipy
RUN pip install seaborn
RUN pip install setuptools
RUN pip install simplegeneric
RUN pip install singledispatch
RUN pip install sip
RUN pip install six
RUN pip install sklearn
RUN pip install slackclient
RUN pip install snowballstemmer
RUN pip install sortedcollections
RUN pip install sortedcontainers
RUN pip install sphinxcontrib-websupport
RUN pip install spyder
RUN pip install statsmodels
RUN pip install sympy
RUN pip install tables
RUN pip install tblib
RUN pip install termcolor
RUN pip install terminado
RUN pip install testpath
RUN pip install toolz
RUN pip install tornado
RUN pip install traitlets
RUN pip install typing
RUN pip install unicodecsv
RUN pip install urllib3
RUN pip install visitor
RUN pip install wcwidth
RUN pip install webencodings
RUN pip install websocket-client
RUN pip install wheel
RUN pip install widgetsnbextension
RUN pip install win-inet-pton
RUN pip install win-unicode-console
RUN pip install wincertstore
RUN pip install wrapt
RUN pip install xlrd
RUN pip install xlwings
RUN pip install xlwt
RUN pip install xmltodict
RUN pip install zict
RUN pip install zope.interface


CMD ["python3"]
