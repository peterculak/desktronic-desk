# How to Turn Your Desktronic Desk into a Smart Desk: Step-by-Step Tutorial

This guide is for anyone who wants to control their Desktronic HomeOne desk with a smartphone but has found that no such feature or product exists for this model. Since you can't buy a ready-made solution, we will build one by interfacing a microcontroller (like an ESP32) directly with the desk's hardware.

We will use a safe "intercept" method that doesn't involve cutting the original desk cables.

---

## Phase 1: The Shopping List (What you need)

1.  **RJ50 (10P10C) Extension/Patch Cable**: You need a cable with 10 pins. Most Ethernet cables are RJ45 (8 pins) and will **not** work. Look specifically for "RJ50 10P10C".
2.  **Multimeter**: Specifically one with **sharp, pointed probes**.
3.  **Wago Connectors (or Wire Nuts)**: For safely tapping into the wires once you cut them.
4.  **Wire Stripper**: Or a careful hand with a utility knife.
5.  **Small Philips Screwdriver**: To open the desk controller box.

---

## Phase 2: Setup (The Breakout)

Before doing anything, **unplug your desk from the wall power**.

1.  Locate the cable connecting your desk motor to the hand controller.
2.  Unplug it.
3.  Connect your **extra RJ50 cable** in between the motor and the controller. 
    *   *Now you have a "disposable" cable that you can cut or strip without ruining your desk's original wiring.*
4.  **Prepare for Probing**:
    *   **Option A (Recommended for next steps)**: Completely cut the extra cable in half and strip all 10 wires on both ends. Reconnect them using **Wago connectors**. This makes it incredibly easy to "tap" into the lines later for your ESP32.
    *   **Option B (Minimalist)**: Only strip a small window of insulation (3-5mm) on the wires you want to test. This keeps the cable intact but is harder to connect to a microcontroller later.

---

## Phase 3: Finding Ground (GND)

You need to find the "Ground" wire. This is your 0V reference point.

1.  **Open the Controller Box**: Use your screwdriver to reveal the PCB (the green board).
2.  **Set your Multimeter**: Turn the dial to **Continuity Mode** (it usually looks like a sound wave symbol •))) ). Touch the two probes together; it should "beep".
3.  **Identify a PCB Ground**:
    *   On the top of the board, look for 4 small metal circles (called "vias") near the RJ50 socket.
    *   Connect your **Black probe** to one of these circles.
4.  **Find the matching wire**:
    *   With the desk still **unpowered**, touch your **Red probe** to each wire in your extra cable one by one.
    *   When the multimeter **beeps**, you found it! 
    *   *In our case, the **Orange** wire was the Ground.*

---

## Phase 4: Mapping the Signals

Now we find which wires move the desk.

1.  **Plug the desk back into the wall**.
2.  **Set your Multimeter**: Turn the dial to **DC Voltage (V⎓)** and set it to the **20V** range.
3.  **Setup Probes (The Wago Shortcut)**:
    *   *Pro Tip: Connecting probes while pressing buttons is tricky. To make it easy, wire ALL 10 wires into Wago connectors first.*
    *   This allows you to safely and reliably "tap" any wire with your probes.
    *   **One-Hand Probing**: Hold both the Black probe (on Orange GND) and the Red probe (on your test wire) in one hand.
    *   **One-Hand Control**: This leaves your other hand completely free to press and hold the desk button.
4.  **Test for UP/DOWN**:
    *   **Idle**: With both probes in one hand, check the voltage (should be 5V).
    *   **Press UP on the desk**: Use your free hand to hold the UP button. If the voltage drops to **0V**, that's your **UP** wire!
    *   **Press DOWN on the desk**: Repeat to find the **DOWN** wire.

**The Discovery**:
- **Purple Wire**: 5V idle → **0V when UP is pressed**.
- **Grey Wire**: 5V idle → **0V when DOWN is pressed**.

---

## Phase 5: Verification (The Short Test)

Once you think you have the right wires, you can do a manual test:

1.  Take a small piece of spare wire.
2.  Briefly touch one end to the **Orange (GND)** wire and the other to the **Purple (UP)** wire.
3.  The desk should move **UP**. 
4.  Repeat with **Grey (DOWN)**; the desk should move **DOWN**.

---

## Phase 6: Conclusion & Next Steps

You now have 3 wires identified:
- **GND**: Orange
- **UP**: Purple
- **DOWN**: Grey

To control this with an **ESP32**, you will need to connect a Relay or an Optocoupler to these three lines. 

*   **If you used Wago connectors**: Simply open the Wago for Orange, Purple, or Grey, and insert your ESP32's control wire alongside the existing desk wire.
*   **If you only identified 3 wires**: You only need to cut/Wago the **Orange, Purple, and Grey** wires. The other 7 wires in the RJ50 cable can remain untouched and uncut.

When you want to move the desk, your ESP32 just needs to "short" Purple or Grey to Orange.

> [!IMPORTANT]
> Always use optocouplers or relays to isolate your ESP32 from the desk controller to prevent damage!
