add_library(TestMain
            TestMain.cpp)
target_link_libraries(TestMain
                      PRIVATE
                        LLVMSupport
                        gtest)

add_executable(BackendCorrectnessTest
               BackendTestUtils.cpp
               BackendCorrectnessTest.cpp)
target_link_libraries(BackendCorrectnessTest
                      PRIVATE
                        Backends
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        Quantization
                        Support
                        gtest
                        TestMain)
add_glow_test(BackendCorrectnessTest
              ${GLOW_BINARY_DIR}/tests/BackendCorrectnessTest --gtest_output=xml:BackendCorrectnessTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/BackendCorrectnessTest -optimize-ir=false &&)

add_executable(BackendTest
               BackendTest.cpp)
target_link_libraries(BackendTest
                      PRIVATE
                        Backends
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(BackendTest ${GLOW_BINARY_DIR}/tests/BackendTest --gtest_output=xml:BackendTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/BackendTest -optimize-ir=false &&)

add_executable(BasicIRTest
               BasicIRTest.cpp)
target_link_libraries(BasicIRTest
                      PRIVATE
                        Graph
                        IR
                        gtest
                        TestMain)
add_glow_test(BasicIRTest ${GLOW_BINARY_DIR}/tests/BasicIRTest --gtest_output=xml:BasicIRTest.xml)

add_executable(Caffe2ImporterTest
               Caffe2ImporterTest.cpp)
target_link_libraries(Caffe2ImporterTest
                      PRIVATE
                        Graph
                        Importer
                        ExecutionEngine
                        gtest
                        TestMain)
add_glow_test(NAME Caffe2ImporterTest
              COMMAND ${GLOW_BINARY_DIR}/tests/Caffe2ImporterTest --gtest_output=xml:Caffe2ImporterTest.xml
              WORKING_DIRECTORY ${CMAKE_BINARY_DIR})

add_executable(ConvTest
               ConvTest.cpp)
target_link_libraries(ConvTest
                      PRIVATE
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        Support
                        gtest
                        TestMain)
add_glow_test(ConvTest
              ${GLOW_BINARY_DIR}/tests/ConvTest)
LIST(APPEND UNOPT_TESTS ./tests/ConvTest -optimize-ir=false &&)

add_executable(DeviceManagerTest
               DeviceManagerTest.cpp)
target_link_libraries(DeviceManagerTest
                      PRIVATE
                        Backend
                        Backends
                        DeviceManager
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(DeviceManagerTest ${GLOW_BINARY_DIR}/tests/DeviceManagerTest --gtest_output=xml:DeviceManagerTest.xml)

add_executable(Float16Test
               Float16Test.cpp)
target_link_libraries(Float16Test
                      PRIVATE
                        Support
                        gtest
                        TestMain)
add_glow_test(Float16Test ${GLOW_BINARY_DIR}/tests/Float16Test --gtest_output=xml:Float16Test.xml)

if(GLOW_WITH_CPU AND NOT MSVC)
  add_executable(GemmTest
                 GemmTest.cpp)
  target_link_libraries(GemmTest
                        PRIVATE
                          CPURuntimeNative
                          Graph
                          IR
                          ExecutionEngine
                          Support
                          gtest
                          TestMain)
  add_glow_test(GemmTest ${GLOW_BINARY_DIR}/tests/GemmTest --gtest_output=xml:GemmLTest.xml)
  LIST(APPEND UNOPT_TESTS ./tests/GemmTest -optimize-ir=false &&)
endif()

add_executable(GlowOnnxifiManagerTest
               GlowOnnxifiManagerTest.cpp)
target_link_libraries(GlowOnnxifiManagerTest
                      PRIVATE
                        onnxifi-glow-lib
                        gtest
                        TestMain)
add_glow_test(NAME GlowOnnxifiManagerTest
              COMMAND ${GLOW_BINARY_DIR}/tests/GlowOnnxifiManagerTest --gtest_output=xml:GlowOnnxifiManagerTest.xml
              WORKING_DIRECTORY ${CMAKE_BINARY_DIR})

add_executable(GradCheckTest
               GradCheckTest.cpp)
target_link_libraries(GradCheckTest
                      PRIVATE
                        Base
                        ExecutionEngine
                        Graph
                        IR
                        gtest
                        TestMain)
add_glow_test(GradCheckTest ${GLOW_BINARY_DIR}/tests/GradCheckTest --gtest_output=xml:GradCheckTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/GradCheckTest -optimize-ir=false &&)

add_executable(GraphGradTest
               GraphGradTest.cpp)
target_link_libraries(GraphGradTest
                      PRIVATE
                        Graph
                        IR
                        ExecutionEngine
                        gtest
                        TestMain)
add_glow_test(GraphGradTest ${GLOW_BINARY_DIR}/tests/GraphGradTest --gtest_output=xml:GraphGradTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/GraphGradTest -optimize-ir=false &&)

add_executable(GraphOptzTest
               GraphOptzTest.cpp)
target_link_libraries(GraphOptzTest
                      PRIVATE
                        Graph
                        IR
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(GraphOptzTest ${GLOW_BINARY_DIR}/tests/GraphOptzTest --gtest_output=xml:GraphOptzTest.xml)

add_executable(GraphSchedulerTest
               GraphSchedulerTest.cpp)
target_link_libraries(GraphSchedulerTest
                      PRIVATE
                        Graph
                        IR
                        gtest
                        TestMain)
add_glow_test(GraphSchedulerTest ${GLOW_BINARY_DIR}/tests/GraphSchedulerTest --gtest_output=xml:GraphSchedulerTest.xml)

add_executable(GraphTest
               GraphTest.cpp)
target_link_libraries(GraphTest
                      PRIVATE
                        Backends
                        ExecutionEngine
                        Graph
                        IR
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(GraphTest ${GLOW_BINARY_DIR}/tests/GraphTest --gtest_output=xml:GraphTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/GraphTest -optimize-ir=false &&)

if(GLOW_WITH_CPU)
  add_executable(HyphenTest
                 BackendTestUtils.cpp
                 HyphenTest.cpp)
  target_link_libraries(HyphenTest
                        PRIVATE
                          Backends
                          Graph
                          IR
                          ExecutionEngine
                          Optimizer
                          Quantization
                          Support
                          gtest
                          TestMain)
  add_glow_test(HyphenTest ${GLOW_BINARY_DIR}/tests/HyphenTest --gtest_output=xml:HyphenTest.xml)
  list(APPEND UNOPT_TESTS ./tests/HyphenTest -optimize-ir=false &&)
endif()

if(PNG_FOUND)
  add_executable(ImageTest
                 ImageTest.cpp)
  target_link_libraries(ImageTest
                        PRIVATE
                          Base
                          gtest
                          TestMain)
  add_glow_test(ImageTest ${GLOW_BINARY_DIR}/tests/ImageTest --gtest_output=xml:ImageTest.xml
                WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
endif()

add_executable(IROptTest
               IROptTest.cpp)
target_link_libraries(IROptTest
                      PRIVATE
                        Backends
                        Graph
                        IR
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(IROptTest ${GLOW_BINARY_DIR}/tests/IROptTest --gtest_output=xml:IROptTest.xml)

if(GLOW_WITH_CPU)
  add_executable(LLVMIRGenTest
                 LLVMIRGenTest.cpp)
  target_link_libraries(LLVMIRGenTest
                        PRIVATE
                          Backends
                          CPUBackend
                          IR
                          Support
                          gtest
                          TestMain)
  add_glow_test(LLVMIRGenTest ${GLOW_BINARY_DIR}/tests/LLVMIRGenTest --gtest_output=xml:LLVMIRGenTest.xml)
endif()

add_executable(MemoryAllocatorTest
               MemoryAllocatorTest.cpp)
target_link_libraries(MemoryAllocatorTest
                      PRIVATE
                        CodeGen
                        gtest
                        Support
                        TestMain)
add_glow_test(MemoryAllocatorTest ${GLOW_BINARY_DIR}/tests/MemoryAllocatorTest --gtest_output=xml:MemoryAllocatorTest.xml)

add_executable(MLTest
               MLTest.cpp)
target_link_libraries(MLTest
                      PRIVATE
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        Quantization
                        gtest
                        TestMain)
add_glow_test(MLTest ${GLOW_BINARY_DIR}/tests/MLTest --gtest_output=xml:MLTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/MLTest -optimize-ir=false &&)

if(GLOW_WITH_OPENCL)
  add_executable(OCLTest
                 BackendTestUtils.cpp
                 OCLTest.cpp)
  target_link_libraries(OCLTest
                        PRIVATE
                          ExecutionEngine
                          Graph
                          IR
                          Optimizer
                          gtest
                          TestMain)
  add_glow_test(OCLTest ${GLOW_BINARY_DIR}/tests/OCLTest --gtest_output=xml:OCLTest.xml)
  LIST(APPEND UNOPT_TESTS ./tests/OCLTest -optimize-ir=false &&)
endif()

add_executable(OnnxImporterTest
               OnnxImporterTest.cpp)
target_link_libraries(OnnxImporterTest
                      PRIVATE
                        Backends
                        Graph
                        Importer
                        ExecutionEngine
                        gtest
                        TestMain)
add_glow_test(NAME OnnxImporterTest
              COMMAND ${GLOW_BINARY_DIR}/tests/OnnxImporterTest --gtest_output=xml:OnnxImporterTest.xml
              WORKING_DIRECTORY ${CMAKE_BINARY_DIR})

add_executable(OperatorTest
               OperatorTest.cpp)
target_link_libraries(OperatorTest
                      PRIVATE
                        Graph
                        IR
                        ExecutionEngine
                        Quantization
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(OperatorTest ${GLOW_BINARY_DIR}/tests/OperatorTest --gtest_output=xml:OperatorTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/OperatorTest -optimize-ir=false &&)

add_executable(PartitionerTest
               PartitionerTest.cpp)
target_link_libraries(PartitionerTest
                      PRIVATE
                        Backends
                        ExecutionEngine
                        Graph
                        IR
                        Partitioner
                        gtest
                        TestMain)
add_glow_test(PartitionerTest ${GLOW_BINARY_DIR}/tests/PartitionerTest --gtest_output=xml:PartitionerTest.xml)

if(GLOW_WITH_CPU)
  add_executable(ProvisionerTest
                 ProvisionerTest.cpp)
  target_link_libraries(ProvisionerTest
                        PRIVATE
                          Backends
                          Graph
                          IR
                          Provisioner
                          DeviceManager
                          CPUDeviceManager
                          gtest
                          TestMain)
  target_include_directories(ProvisionerTest PUBLIC ${CMAKE_SOURCE_DIR}/lib/Backends/CPU)
  add_glow_test(ProvisionerTest ${GLOW_BINARY_DIR}/tests/ProvisionerTest --gtest_output=xml:ProvisionerTest.xml)

add_executable(HostManagerTest
               HostManagerTest.cpp)
target_link_libraries(HostManagerTest
                      PRIVATE
                        Backends
                        HostManager
                        Executor
                        Graph
                        IR
                        Partitioner
                        Provisioner
                        DeviceManager
                        gtest
                        TestMain)
target_include_directories(HostManagerTest PUBLIC ${CMAKE_SOURCE_DIR}/lib/Backends)
add_glow_test(HostManagerTest ${GLOW_BINARY_DIR}/tests/HostManagerTest --gtest_output=xml:ProvisionerTest.xml)

endif()

add_executable(QuantizationTest
               QuantizationTest.cpp)
target_link_libraries(QuantizationTest
                      PRIVATE
                        Backends
                        ExecutionEngine
                        Graph
                        Optimizer
                        Quantization
                        gtest
                        TestMain)
add_glow_test(QuantizationTest ${GLOW_BINARY_DIR}/tests/QuantizationTest --gtest_output=xml:QuantizationTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/QuantizationTest -optimize-ir=false &&)

add_executable(ExecutorTest
               ExecutorTest.cpp)
target_link_libraries(ExecutorTest
                      PRIVATE
                        DeviceManager
                        Executor
                        Graph
                        gtest
                        TestMain
                        ThreadPool)
add_glow_test(ExecutorTest ${GLOW_BINARY_DIR}/tests/ExecutorTest --gtest_output=xml:ExecutorTest.xml)

add_executable(TensorsTest
               TensorsTest.cpp)
target_link_libraries(TensorsTest
                      PRIVATE
                        Base
                        gtest
                        TestMain)
add_glow_test(TensorsTest ${GLOW_BINARY_DIR}/tests/TensorsTest --gtest_output=xml:TensorsTest.xml)

add_executable(ThreadPoolTest
               ThreadPoolTest.cpp)
target_link_libraries(ThreadPoolTest
                      PRIVATE
                        ThreadPool
                        gtest
                        TestMain)
add_glow_test(ThreadPoolTest ${GLOW_BINARY_DIR}/tests/ThreadPoolTest --gtest_output=xml:ThreadPoolTest.xml)

add_executable(TraceEventsTest
               TraceEventsTest.cpp)
target_link_libraries(TraceEventsTest
                      PRIVATE
                        Backend
                        Backends
                        Graph
                        IR
                        ExecutionEngine
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(TraceEventsTest ${GLOW_BINARY_DIR}/tests/TraceEventsTest --gtest_output=xml:TraceEventsTest.xml)
LIST(APPEND UNOPT_TESTS ./tests/TraceEventsTest -optimize-ir=false &&)

add_executable(TypeAToTypeBFunctionConverterTest
               TypeAToTypeBFunctionConverterTest.cpp)
target_link_libraries(TypeAToTypeBFunctionConverterTest
                      PRIVATE
                        Converter
                        ExecutionEngine
                        Graph
                        Optimizer
                        gtest
                        TestMain)
add_glow_test(TypeAToTypeBFunctionConverterTest
              ${GLOW_BINARY_DIR}/tests/TypeAToTypeBFunctionConverterTest --gtest_output=xml:TypeAToTypeBFunctionConverterTest.xml)

add_executable(UtilsTest
               StrCheck.cpp
               SupportTest.cpp
               TaggedListTest.cpp
               UtilsTest.cpp)
target_link_libraries(UtilsTest
                      PRIVATE
                        Base
                        Support
                        Testing
                        gtest
                        TestMain)
add_glow_test(UtilsTest ${GLOW_BINARY_DIR}/tests/UtilsTest --gtest_output=xml:UtilsTest.xml)

LIST(APPEND UNOPT_TESTS true)
add_custom_target(test_unopt ${UNOPT_TESTS}
                  WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
