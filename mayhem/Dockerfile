# Copyright 2019 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder

RUN apt-get update && apt-get install -y autoconf zlibc libtool liblcms2-dev libpng-dev
RUN git clone --branch branch-2.2 --single-branch --depth 1 https://github.com/apple/cups.git cups
RUN git clone --branch VER-2-12-1 --single-branch --depth 1 https://git.savannah.gnu.org/git/freetype/freetype2.git freetype
COPY . ghostpdl
RUN rm -rf ghostpdl/mayhem
RUN mkdir ghostpdl/fuzz

COPY mayhem/pdf_seeds $SRC/pdf_seeds
COPY mayhem/dicts $SRC/dicts

WORKDIR ghostpdl

COPY mayhem/*.cc mayhem/*.options mayhem/*.h $SRC/
COPY mayhem/build.sh $SRC/

ENV FUZZING_LANGUAGE=c++ SANITIZER=address FUZZING_ENGINE=libfuzzer
RUN compile
RUN chmod +x /out/gs_device_eps2write_fuzzer
