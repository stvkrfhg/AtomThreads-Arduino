---------------------------------------------------------------------------

 * Library: Atomthreads
 * Author: Kelvin Lawson <info@atomthreads.com>
 * Website: http://atomthreads.com
 * License: BSD Revised

---------------------------------------------------------------------------

Atomthreads is a free RTOS for embedded systems, released under the
flexible, open source BSD license and is free to use for commercial or
educational purposes without restriction.

It is targeted at systems that need only a lightweight scheduler and the
usual RTOS primitives. No file system, IP stack or device drivers are
included, but developers can bolt on their own as required. Atomthreads
will always be a small number of C files which are easy to port to any
platforms that require threading by adding a simple
architecture-specific file.

---------------------------------------------------------------------------

DOCUMENTATION:

All documentation is contained within the source files, commented using
Doxygen markup. Pre-generated documentation can be accessed at
http://atomthreads.com.

See also the README file contained within each folder of the source tree.

---------------------------------------------------------------------------

GETTING START

```arduino
#include <AtomThreads.h>
#define IDLE_STACK_SIZE_BYTES 128
#define MAIN_STACK_SIZE_BYTES 204
#define DEFAULT_THREAD_PRIO 16
static ATOM_TCB main_tcb;
static uint8_t main_thread_stack[MAIN_STACK_SIZE_BYTES];
static uint8_t idle_thread_stack[IDLE_STACK_SIZE_BYTES];
uint8_t status;

void setup(){
    SP = (int)&idle_thread_stack[(IDLE_STACK_SIZE_BYTES/2) - 1];
    status = atomOSInit(&idle_thread_stack[0], IDLE_STACK_SIZE_BYTES, FALSE);
    if (status == ATOM_OK) {
        avrInitSystemTickTimer();
        status = atomThreadCreate(&main_tcb,
                     DEFAULT_THREAD_PRIO, main_thread_func, 0,
                     &main_thread_stack[0],
                     MAIN_STACK_SIZE_BYTES,
                     FALSE);
        if (status == ATOM_OK) {
            atomOSStart();
        }
    }
}

static void main_thread_func (uint32_t) {
    // initialize digital pin LED_BUILTIN as an output.
    pinMode(LED_BUILTIN, OUTPUT);
    while (1) {
        loop();
    }
}

// Loop and print the time every second.
void loop() {
    digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
    atomTimerDelay(SYSTEM_TICKS_PER_SEC);                       // wait for a second
    digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
    atomTimerDelay(SYSTEM_TICKS_PER_SEC);
}
```
