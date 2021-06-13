ARG BASE_IMAGE_DISTRO=casperfrx/intel-oneapi-cpp-python-devel
ARG BASE_IMAGE_VERSION=centos8-2021.2.0

FROM "$BASE_IMAGE_DISTRO":"$BASE_IMAGE_VERSION"

ARG MAKE_COMPILER_CORE_COUNT_MULTIPLIER=1

RUN true \
    &&  dnf install -y \
            epel-release \
    &&  dnf install -y \
            git \
            unzip \
            zip \
            curl \
            openssl \
            libgfortran \
            libgomp \
            R-core-devel \
            libicu-devel \
            libcurl-devel \
            openssl-devel \
            libxml2-devel \
    &&  dnf config-manager --set-enabled powertools \
    &&  dnf clean all \
    &&  rm -rf \
        /{tmp,var/tmp}/* \
    &&  true

RUN true \
    &&  mkdir -p \
        /usr/local/lib/R/site-library \
    &&  R --slave --no-restore --method=curl --Ncpus="$(expr $(nproc) \* ${MAKE_COMPILER_CORE_COUNT_MULTIPLIER})" --clean \
        -e "install.packages(c('httr', 'RJSONIO', 'dplyr','curl', 'IRkernel', 'sparklyr','ggplot2', 'gtools', 'tidyr', 'rmarkdown','base64enc', 'filelock'),'/usr/local/lib/R/site-library',repos='https://cloud.r-project.org')" \
    &&  rm -rf \
        /{tmp,var/tmp}/* \
    &&  true
