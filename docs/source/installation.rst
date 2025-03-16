SYCL Installation Guide
=====

.. _Windows WSL2 Ubuntu / Ubuntu:

Ubuntu Installation Guide:
--------------------------

Using AdaptiveCpp (Formely Open SYCL):

.. code-block:: console

    sudo -i

    apt upgrade && apt upgrade -y

    apt-get update && apt-get install -y git cmake g++ make libboost-all-dev ninja-build python3 python3-pip wget curl lsb-release gnupg software-properties-common llvm clang libclang-dev

    git clone https://github.com/AdaptiveCpp/AdaptiveCpp.git

    cd AdaptiveCpp

    mkdir build && cd build

    cmake ..

    make install -j ${nproc}

Using Intel OneAPI (https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=oneapi-toolkit&oneapi-toolkit-os=linux&oneapi-lin=apt):

.. code-block:: console

    sudo apt update

    sudo apt install -y gpg-agent wget

    wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \
    | gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null

    echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneAPI.list

    sudo apt update

    sudo apt install intel-oneapi-base-toolkit 

    sudo apt update

    sudo apt -y install cmake pkg-config build-essential

To ensure that all paths are added to environment run:

.. code-block:: console

    ./opt/intel/oneapi/<toolkit-version>/oneapi-vars.sh

    icpx --version

Every time you start a session or add it to bash profile by:

.. code-block:: console

    nano ~./bashrc

    ./opt/intel/oneapi/<toolkit-version>/oneapi-vars.sh > /dev/null

