---
layout: post
title: "ST external loader learning"
tags:
  - english
  - ST
  - microcontroller
  - documentation
---

I want to share some knowledge about constructing an external loader for an ST project. After long days, undergoing mysterious errors and bad documentation, I finally built a functional loader. I will not teach how to do it but simply explain how it works and point some issues that I came across (my experience =)).

<!-- more -->

The external loader, although it is a generic name and may refer to a lot of things, in this context is a feature of the st-link, the programmer of ST microcontrollers.

It's essentially a firmware to interface with external memory, usually a NOR or NAND flash of your board. The scenario is the st-link connected to the microcontroller by the programmer interface and the microcontroller connected to the external memory by any interface. The programmer does not directly access the memory itself. All the memory operations need to pass through the microcontroller.

![placeholder](/assets/images/2021-04-XX-external_loader/external-loader-diagram.png "Diagram shown in the ST videos")

The problem is that the programmer does not know how the memory and the microcontroller are connected. There are many memory interface (eMMC, SPI, QSPI, OSPI, FMIC, ...) possibilities, pin configurations, clock, power management, lot of options to connect both. It is very close to hardware design.

ST has solved this problem with its external loader. It is a Firmware loaded into the microcontroller and "communicate" with the programmer to perform memory operations.

This Firmware is designed with the appropriate configuration to access the memory, providing functions to perform the operations by the programmer.

The general flow when programming the board is by st-link loading the external loader application to the microcontroller RAM. Then it performs all the operations needed. Finally, free the memory and continue the board programmer sequence, as we already know.

They have created some documentation and provided it in the [ST](https://www.st.com/content/st_com/en/support/learning/stm32-education/stm32-moocs/external_QSPI_loader.html). One of the documentations is a series of videos on [Youtube](https://www.youtube.com/playlist?list=PLnMKNibPkDnHIrq5BICcFhLsmJFI_ytvE) teaching and constructing an external loader. The videos give a good idea about how it works, but they left some missing points. Also, the example they provide is limited, as the case is an already know board that suits perfectly in the code/base provided on [git](https://github.com/STMicroelectronics/stm32-external-loader/). But still worth the time watching and reading. Besides this video and the git repository, there are no more than two pages in the programmer [documentation](https://www.st.com/content/ccc/resource/technical/document/user_manual/e6/10/d8/80/d6/1d/4a/f2/CD00262073.pdf/files/CD00262073.pdf/jcr:content/translations/en.CD00262073.pdf).

The most important thing to know is that the external loader doesn't run when loaded into the microcontroller RAM. The programmer sets what it wants to run. So, the Firmware provides no communication. Therefore it is one less thing to worry about.
 The image below shows this. It is verbosity 3 of the programmer log.

![placeholder](/assets/images/2021-04-XX-external_loader/logProgrammer.png "STM32CubeProgrammer log")

In order to execute the function, the programmer sets the core registers and runs. The PC (program counter), R0 to RX (functions arguments), and other Registers are set to their values. In the end, it runs and gets the return value.

Thus, the external loader needs to provide a set of functions that the programmer will use. These functions are "documented" in one of the two pages. Some functions are mandatory and cannot be omitted.
All the example provided in the repository is for mapped memory, so if you use a non-memory mapped device you need to implement an optional function, Read.

The programmer finds these functions because the external loader is an elf file. The file extension is changed to ".stldr".

![placeholder](/assets/images/2021-04-XX-external_loader/writeFunction.png "Output of readelf cmd")

# Problems and solutions?

This section is to provide some facts that I discovered and some problems I faced.

According to the documentation, you need to store a struct called StorageInfo. This structure has some information about the external loader, like name, type, memory start address, size, and page size.
But it seems that this is just for visualization.
I hoped that if you tell the programmer that you use a NAND flash, you expect that it respects the NAND Flash architecture, sends whole pages to be written and read, boundary address, right? But, this doesn't happen. So you need to deal with un-boundary access and any size on the low level.

One mysterious problem I had, was with local variables. For some unknown reason, sometimes it just stopped working. After some unsuccessful debug, I just loaded it as global variables and, it worked well (until now, at least).

I hope this can help someone. If someday I find out, I will update here =)

