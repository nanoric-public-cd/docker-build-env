FROM centos:7.7.1908
  
RUN yum -y update && yum install -y epel-release centos-release-scl && yum install -y devtoolset-9 git vim boost openssl openssl-devel openssl-static tbb tbb-devel mesa-libGL-devel
RUN yum -y install rh-git218 python3 cmake3 && ln -s /usr/bin/cmake3 /usr/bin/cmake

ENV LD_LIBRARY_PATH=/opt/rh/httpd24/root/usr/lib64:/opt/rh/devtoolset-9/root/usr/lib64:/opt/rh/devtoolset-9/root/usr/lib:/opt/rh/devtoolset-9/root/usr/lib64/dyninst:/opt/rh/devtoolset-9/root/usr/lib/dyninst:/opt/rh/devtoolset-9/root/usr/lib64:/opt/rh/devtoolset-9/root/usr/lib:$LD_LIBRARY_PATH
ENV PATH=/opt/rh/rh-git218/root/usr/bin:/opt/rh/devtoolset-9/root/usr/bin:$PATH
ENV PKG_CONFIG_PATH=/opt/rh/devtoolset-9/root/usr/lib64/pkgconfig:$PKG_CONFIG_PATH
RUN git clone https://github.com/microsoft/vcpkg && cd vcpkg && ./bootstrap-vcpkg.sh
RUN cd vcpkg && ./vcpkg --triplet x64-linux install --head date range-v3 scnlib cppzmq
RUN cd vcpkg && ./vcpkg --triplet x64-linux install boost openssl tbb yaml-cpp spdlog gtest
ENV VCPKG_CMAKE_TOOLCHAIN_FILE /vcpkg/scripts/buildsystems/vcpkg.cmake

RUN git clone https://github.com/Kitware/CMake.git && cd CMake && git checkout release && ./bootstrap && make -j8 && make install && cd ../../ && rm -rf CMake
RUN git clone https://github.com/ninja-build/ninja.git && cd ninja && git checkout release && cmake -Bbuild -H. && cd build && make -j8 && cp -f ninja /usr/local/bin && cd ../../ && rm -rf ninja
