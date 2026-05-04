---
title: Reflection
---

## Review of Module's Sucess

The module succeeded in meeting all of it's target goals across multiple boards. Due to defects in the boards and voltage regulators, power-related goals were only met on one board, and bluetooth goals were met on another board. With a breakout, the final demo was able to meet all the requirements.

## ESP32 Tips:
- If your ESP32 with micropython is unable to upload code after a bad program, try the following commands (requires esptool):
    - erase flash
    - rewrite flash
    - zero the memory address a few bytes after the firmware ends
    - rewrite flash
    - esptool run
- On desktop Linux systems (personally using Debian 13), Pymakr will not work, as it is severely outdated. A more flexible and standard workflow can be obtained by writing simple shell scripts around the mpremote tool. You can use your text editor/IDE of choice to write python code and use mpremote or your shell scripts from the terminal.
    - mpremote allows you to mount your local directory to the esp32. This is very useful for testing, since you don't need to copy anything.
    - mpremote ls/cp/rm commands follow standard posix syntax.
- Leaving IO0 floating will cause the ESP32 to believe it is in boot mode. Pull this pin high (with a jumper or a switch).
- Do not use RXD0/TXD0 as your UART pins. These will recieve hardware interrupts and result in strange behaviours.
- micropython sometimes throws errors with nested directories, even if a path is clearly specified. Use flat directory structures to minimize errors.
- Overall, *use C on the ESP32* if possible! This project used micropython to make use of the aioble library. If you do not need a wireless function, C will make life much easier.
- If you use asyncio with micropython, note that try-catches behave in slightly different ways compared to C or C++. Read the documentation about these carefully.
- If your ESP makes a high-pitched whining noise, add extra decoupling capacitors.

## Lessons Learned

1. Account for power regulation to fail, and add breakout headers in advance for this to supply power to test/debug.
2. I learnt a lot more about desoldering than I've had to in the past.

## Recommendations for Future Students

1. *Standardize as many components as you can across your team so you all have spares!!!*
2. Build your board with sufficient options to solder/jump functionality if something fails.
3. Build your team's system so your project isn't reliant on a single board to show functionality. Eg, our team relied on a display on our HMI board, which was not working during the demo. We instead showed sensor data through connecting laptops to the bluetooth relay and camera boards. We had multiple boards fail to regulate power, so 3.3V power was supplied to them through pins from the battery breakout board.
4. If you're working in a large team, split up into subteams early. Our team had 10 people, which made coordinating as a full group difficult. However, we had small subteams that could act largely independantly of eachother, so this was not an issue.
5. *Review all of your PCBs in a team. Your teammates will catch a lot of things you didn't, and vice-versa!!!*
6. *version control is your friend!!!* Using a version control system of your choice (git is the most common) will make managing different versions of your software much easier. You can rollback and track changes, which is *much* easier than maintaining different files in different places. Please take the time to learn this at the start. A local repository is fine. You will thank yourself by the end!
