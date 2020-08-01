# Intel Modular Scatter-Gatter Direct Memory Access engine software wrapper

This directory contains a software library to ease the process of configuring and controlling the Intel Modular Scatter-Gatter Direct Memory Access (MSGDMA) engine. It is compatible with both bare-metal and Linux development.

**This library was developed by [Sahand Kashani](https://github.com/sahandKashani).**

To get familiar with the API of this library, I recommend reading the header file [msgdma.h](msgdma.h). An example of usage is provided below.

1. Create a device descriptor for the MSGDMA engine using, for example, the following function.
```C
msgdma_dev msgdma_csr_descriptor_inst(void *csr_base,
                                      void *descriptor_base,
                                      uint32_t descriptor_fifo_depth,
                                      uint8_t csr_burst_enable,
                                      uint8_t csr_burst_wrapping_support,
                                      uint32_t csr_data_fifo_depth,
                                      uint32_t csr_data_width,
                                      uint32_t csr_max_burst_count,
                                      uint32_t csr_max_byte,
                                      uint64_t csr_max_stride,
                                      uint8_t csr_programmable_burst_enable,
                                      uint8_t csr_stride_enable,
                                      uint8_t csr_enhanced_features,
                                      uint8_t csr_response_port);
```
2. Initialize the device like so.
```C
msgdma_init(msgdma_dev *dev);

```
3. Create a descriptor for the transaction you wish to implement using, for instance, the following routine.
```C
msgdma_standard_descriptor desc;
int msgdma_construct_standard_mm_to_mm_descriptor(msgdma_dev *dev,
                                                  msgdma_standard_descriptor *desc,
                                                  void *read_address,
                                                  void *write_address,
                                                  uint32_t length,
                                                  uint32_t control);
```
4. Start the data transfer synchronously or asynchronously using one of the routines below.
```C
int msgdma_standard_descriptor_async_transfer(msgdma_dev *dev, msgdma_standard_descriptor *desc);
int msgdma_standard_descriptor_sync_transfer(msgdma_dev *dev, msgdma_standard_descriptor *desc);
```
5. Wait until the transaction finishes.
```C
void msgdma_wait_until_idle(msgdma_dev *dev);
```