# Copyright (c) 2017, 2024, Oracle and/or its affiliates.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License, version 2.0,
# as published by the Free Software Foundation.
#
# This program is designed to work with certain software (including
# but not limited to OpenSSL) that is licensed under separate terms,
# as designated in a particular file or component or in included license
# documentation.  The authors of MySQL hereby grant you an additional
# permission to link the program and your derivative works with the
# separately licensed software that they have either included with
# the program or referenced in the documentation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License, version 2.0, for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301  USA

DISABLE_MISSING_PROFILE_WARNING()

ADD_LIBRARY(library_mysys STATIC
  my_memory.cc
  my_hex_tools.cc
  my_base64_encode.cc
  option_usage_data.cc
)

IF(UNIX)
TARGET_COMPILE_OPTIONS(library_mysys PRIVATE "-fvisibility=hidden")
ENDIF()

# Workaround to avoid "cannot allocate memory in static TLS block" error
STRING(REPLACE "-ftls-model=initial-exec" "-ftls-model=global-dynamic"
  CMAKE_C_FLAGS "${CMAKE_C_FLAGS}"
  )
STRING(REPLACE "-ftls-model=initial-exec" "-ftls-model=global-dynamic"
  CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}"
  )

TARGET_LINK_LIBRARIES(library_mysys rapidjson)
