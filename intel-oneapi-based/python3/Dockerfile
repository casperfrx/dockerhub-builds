ARG BASE_IMAGE_DISTRO=casperfrx/intel-oneapi-cpp-python-devel
ARG BASE_IMAGE_VERSION=centos8-2021.2.0

FROM "$BASE_IMAGE_DISTRO":"$BASE_IMAGE_VERSION"

ARG MAKE_COMPILER_CORE_COUNT_MULTIPLIER=1
ARG BUILD_PYTHON_SKIP_COMPILE_TESTS
ARG BUILD_PYTHON_VERSIONS=3.6.13,3.8.10,3.9.5

RUN true \
    &&  dnf install -y \
            epel-release \
    &&  dnf install -y \
            git \
            unzip \
            zip \
            curl \
            diffutils \
            openssl \
            libgfortran \
            libgomp \
            openssl-devel \
            bzip2-devel \
            sqlite-devel \
            zlib-devel \
            expat-devel \
            readline-devel \
    &&  dnf clean all \
    &&  rm -rf \
        /{tmp,var/tmp}/* \
    &&  true

RUN true \
    &&  set -o pipefail \
    &&  for i in $(echo $BUILD_PYTHON_VERSIONS | sed "s/,/ /g"); \
        do \
            echo "Building Python $i" \
            &&  curl -sS \
                    https://www.python.org/ftp/python/"$i"/Python-"$i".tgz \
                | tar -zxC /tmp \
            &&  cd /tmp/Python-"$i" \
            &&  ./configure \
                    --enable-optimizations \
                    --with-lto \
                    --with-system-expat \
                    --with-system-ffi \
                    --with-computed-gotos \
                    --with-threads \
                    --enable-loadable-sqlite-extensions \
                    --with-ensurepip=yes \
            &&  if [[ -z "$BUILD_PYTHON_SKIP_COMPILE_TESTS" ]]; then \
                    make -j "$(expr $(nproc) \* ${MAKE_COMPILER_CORE_COUNT_MULTIPLIER})"; \
                else \
                    make -j "$(expr $(nproc) \* ${MAKE_COMPILER_CORE_COUNT_MULTIPLIER})" build_all; \
                fi \
            &&  make -j "$(expr $(nproc) \* ${MAKE_COMPILER_CORE_COUNT_MULTIPLIER})" altinstall \
            &&  cd  .. \
            &&  rm -rf \
                /{tmp,var/tmp}/*; \
        done \
    &&  strip /usr/local/lib/libpython{3.6m.a,libpython3.8.a,libpython3.9.a}  \
    &&  true
