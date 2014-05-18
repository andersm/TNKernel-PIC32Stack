TNKernel-PIC32Stack
=================

An experimental version of [TNKernel-PIC32](https://github.com/andersm/TNKernel-PIC32 "TNKernel-PIC32") featuring hardware stack overflow protection.

---
##Usage
To use TNKernel-PIC32Stack, set up your project as you would a normal TNKernel-PIC32 project, then just add the included linker script (*tn_port_linkerscript.x*).
##Notes and limitations
The **BMXERRDS** bit in the bus matrix configuration registers must not be cleared.  
Due to hardware limitations, all thread stack allocations will be rounded up to to the nearest kilobyte.  
The memory usage gauge in MPLAB X will not accurately report the amount of free memory. Because of how the XC32 linker allocates sections, all unused memory will be allocated to the thread stack memory region.  
When debugging, the first 512 bytes of RAM are reserved by the debug executive, rendering the interrupt stack protection ineffective.

##Implementation
The stack overflow protection works by making sure that accesses beyond the stack limits will trigger an exception. This is implemented in two ways:  

*   The interrupt stack is allocated at the start of the KSEG1 data RAM region. Overflowing the interrupt stack will access unmapped memory at the top of KSEG0, triggering an exception.
*   The thread stacks are grouped together at the top of RAM. At runtime, the current thread's stack is mapped as user data RAM using the PIC32 bus matrix registers. Overflowing the thread stack will access unmapped USEG memory, again triggering an exception.

---

The kernel is released under the BSD license as follows:

    TNKernel real-time kernel

    Copyright © 2004, 2013 Yuri Tiomkin
    PIC32 version modifications copyright © 2013, 2014 Anders Montonen
    All rights reserved.

    Permission to use, copy, modify, and distribute this software in source
    and binary forms and its documentation for any purpose and without fee
    is hereby granted, provided that the above copyright notice appear
    in all copies and that both that copyright notice and this permission
    notice appear in supporting documentation.

    THIS SOFTWARE IS PROVIDED BY THE YURI TIOMKIN AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
    IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
    ARE DISCLAIMED. IN NO EVENT SHALL YURI TIOMKIN OR CONTRIBUTORS BE LIABLE
    FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
    DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
    OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
    HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
    LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
    OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
    SUCH DAMAGE.
