## 🌟 Light Blink Pro: State Restoration, Stuck Light Recovery, and Programmatic Control
This blueprint provides a highly reliable and feature-rich way to make one or more lights blink for alerts, notifications, or visual feedback. It ensures lights always return to their original state and is designed for maximum compatibility across various light types.

### Core Features
- Essential Blink Functionality: Easily configure lights to flash a specific number of times, at a defined duration, brightness, and transition speed.
- Single Required Input: The light entity itself is the only required input. All other values (count, duration, color, brightness, etc.) are optional and will fall back to their default values if left blank.
- Universal Light Compatibility: Handles blinking for color-capable lights (applying the selected hue) and non-color lights (applying only brightness and timing).
- Flexible Color Selection: Supports choosing a single blink color using either the RGB color picker (which takes priority) or a color name (via text input).
- Reliable State Preservation: The script automatically saves the light's exact state (on/off, brightness, color) to a temporary scene and ensures it is fully restored after the blinking sequence.
- Stuck Light Recovery: Includes logic to detect and recover color lights that fail to restore their original state (a common issue with some light devices).
- Programmatic Override: All configuration details (lights, count, color, duration, etc.) can be passed when calling the script from an automation or another script. This allows a single blueprint installation to handle countless unique scenarios.

### How the Blink Sequence Works
When triggered, the script executes the following process:
- Turn ON Lights: Target lights in the OFF state are turned ON to allow capture of their current attributes that show as null when in the OFF state.
- State Snapshot: Creates a temporary scene to capture the exact current state of all targeted lights.
- Color Determination: Determines the blink color, prioritizing the RGB value from the picker over a typed color name.
- Repeat Loop: Runs for the defined Blink Count. In each cycle, it:
  - Turns the lights ON, applying the color, brightness, and transition speed.
  - Pauses for the specified Blink Duration.
  - Turns the lights OFF or restores their original state.  (Always restores state on the final blink)
  - Pauses again for the Blink Duration.
- Proactive Fallback (Optional): If enabled, the script applies the fallback color/temperature immediately before the final scene restore to proactively clear any residual blink color from the light's hardware memory.
- Final Cleanup: After all recovery and stuck-light checks are complete, any lights that were originally OFF are explicitly turned back OFF

### Built-in Reliability (Stuck Light Recovery)
This multi-stage recovery addresses lights that occasionally get "stuck" on the final blink color:
- Detection: After the primary restoration, the script checks if any light is still stuck on the blink color.
- Recovery Process 1 (Scene Restore): If stuck lights are found, the script makes 2 attempts to apply the original scene 1 second apart (if enabled).
- Recovery Process 2 (Color Fallback): If the light remains stuck, the script applies the Color Fallback Temperature and Brightness Percentage to reset the light to a usable state (if enabled). If the first fallback attempt fails, it will make a second attempt 1 second later to apply the fallback for especially stubborn lights.

This process ensures that your lights always return to a predictable, working state, preventing unexpected permanent color changes after a notification.

### 💡 Timing Tips and Latency
When setting times, remember that **devices need time to communicate and process commands** (latency). If you are seeing issues like wrong count of blinks or missing scene resets, begin slowing increasing these until you see stability.
- **Blink Duration (`input_blink_duration`):** The time the light is ON or OFF. Recommend **1.0 second or more** for reliable blinking on most setups.
- **Wait Time (`input_wait_time_for_light_updates`):** This value is critical for **Stuck Light Recovery**. If your lights often get stuck on the blink color, **increase this time** (e.g., from 1000ms to 2000ms) to give the light more time to fully process the restore command before the script checks its state.
