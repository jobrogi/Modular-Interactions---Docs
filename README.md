# 📦 Modular Interactions Plugin


-[DISCORD SUPPORT LINK HERE](https://discord.gg/YvME6jG8)

-[FAB LINK HERE]()


A highly customizable interaction system built for Unreal Engine 5. This plugin provides developers with a modular, extensible foundation for adding interaction logic to actors, complete with dynamic widgets, reusable behavior types, and clean C++/Blueprint integration.

---

## 🔧 Features

- 🧩 Modular parent-class system with built-in interaction logic
- 🧠 Enum-driven behavior routing (`EActionType`, `EWidgetType`)
- 🎛 Dynamic details panel with conditional properties
- 💡 Supports both native and custom UMG widget prompts
- 🎮 Input mapping with Enhanced Input support
- 🧱 Reusable timeline-based actor movement/rotation
- 🔁 One-click setup with previewable widget actor in editor
- 💙 Fully Blueprint-compatible with override hooks for custom logic

---

## 🚀 Getting Started

### 1. Enable Plugin

Go to `Edit > Plugins` and enable the **Modular Interactions** plugin.

---

### 2. Add the Interaction Handler to Your Character

Add the `BP_InteractionHandlerComponent` to **your main character**.  
This component is required and handles most of the core interaction logic. It must be added directly to the player’s character so the plugin can consistently reference it during interaction.

Once the component is added, you're ready to begin setting up interactables:

- Use `AInteractableActorBase` to define objects that can be interacted with.
- Use `AInteractionRelayActor` to control one or more interactables remotely — great for switches, buttons, and other external triggers.

📌 *See the image below for a reference of available classes.*
IE: `InteractableActorBase`, `InteractionRelayActor`

![InteractBPVisual](https://github.com/user-attachments/assets/1839f81b-7fc2-4081-ae07-531d29983d12)

---

### 3. Configure Interaction Settings

Each interactable actor includes a modular, dynamic details panel for setup:

- **Widget Type**: Choose from Tooltip, Icon Prompt, Radial Progress, or Custom.
- **Action Type**: Choose from Widget Action, Toggle Actor, Custom Event, and more.

🔄 The panel dynamically updates based on your selections:

- Enabling **Use Built-in Action** exposes pre-wired logic like widget toggling.
- Selecting different **Widget Types** reveals widget-specific options.
- Enabling **Use Animation** displays animation-specific settings.

This system keeps clutter to a minimum and only shows settings that are relevant to your current configuration.

---

### 4. Relay Interactable Actors (Optional)

Relay actors (`AInteractionRelayActor`) are designed to **remotely trigger other interactables**.  
These are ideal for buttons, control panels, switches, and similar use cases.

Relay-specific settings allow you to:

- Link multiple target actors
- Automatically follow or attach to another actor
- Control how it aligns and snaps in the world

📌 *See the image below for an example of relay actor configuration.*

![InteractionRelaySettings](https://github.com/user-attachments/assets/20d4fc19-c5cd-410a-8a11-f033c7e7ea95)

---

### 5. Hook Up Inputs

Make sure your character has the correct input mappings via **Enhanced Input**.  
Alternatively, you can directly call `FinalInteract()` in custom input setups.

---

## ⚙️ Interactable Actor Settings

These settings are available when working with `AInteractableActorBase`. The panel is dynamic and changes based on selected enum options like `Action Type` and `Widget Type`.

---

### 🧩 Main Settings

| Property | Type | Description |
|---------|------|-------------|
| `bUseBuiltInAction` | `bool` | Enables built-in behavior (e.g., toggle actor, show widget). Disabling lets you implement your own logic. |
| `ActionType` | `EActionType` | Defines which behavior to run when interacted with. Options include `WidgetAction`, `ToggleActor`, `TimelineMove`, etc. |
| `bUseAnimation` | `bool` | Enables animation montage playback during interaction. |
| `AnimationToPlay` | `UAnimMontage*` | The animation to play if `bUseAnimation` is enabled. |
| `bCanMoveDuringMontage` | `bool` | Allows player movement during animation. |
| `bFullBodyAnim` | `bool` | Plays the animation as a full-body montage. |
| `bControlledByOtherInteractable` | `bool` | When true, this actor is triggered remotely (e.g., by a relay actor). |
| `bRequiresManualTrigger` | `bool` | Actor must be triggered manually rather than by direct player input. |
| `WidgetType` | `EWidgetType` | Specifies which visual interaction widget to show (e.g., tooltip, radial progress, icon). |
| `CustomWidgetClass` | `TSubclassOf<UUserWidget>` | If `WidgetType == CustomWidget`, use this to assign a custom UMG class. |
| `WidgetOffset` | `FVector` | Offset position of the widget relative to the actor. |
| `WidgetScale` | `FVector` | Controls the 3D size of the widget when in world space. |
| `ScreenSpaceType` | `EWidgetSpace` | Determines if the widget is rendered in screen or world space. |
| `bChangeWidgetPitch` | `bool` | Allows the widget to rotate in pitch (world space only). |
| `bChangeWidgetRoll` | `bool` | Allows the widget to rotate in roll (world space only). |
| `bChangeWidgetYaw` | `bool` | Allows the widget to rotate in yaw (world space only). |
| `bShowWidgetShadows` | `bool` | Enables shadow rendering on the widget. |
| `InteractText` | `FString` | Text shown inside radial progress widgets. |
| `InteractToolTip` | `FString` | Text used for tooltip-style widgets. |
| `Icon` | `UTexture2D*` | Image used in icon-based prompts. |
| `WidgetToOpen` | `TSubclassOf<UUserWidget>` | Widget to open when `ActionType == WidgetAction`. |
| `TargetActorToToggle` | `AActor*` | Used for `ToggleActor` type to toggle visibility or state. |
---

### 🎬 Animation

| Property | Description |
|---------|-------------|
| **Use Animation** | Enables animation-related settings (e.g., montage playback). More settings will appear once this is enabled. |

---

### 🖼 Widget Settings

| Property | Description |
|---------|-------------|
| **Widget Type** | The visual style for interaction. Options include `Tooltip`, `Radial Progress`, `Press Prompt`, `Icon`, and `Custom`. |
| **Custom Widget Class** | When `Widget Type` is set to `Custom`, assign your custom `UUserWidget` class here. |
| **Widget Offset** | 3D offset of the widget in world or screen space. |
| **Widget Scale** | Scale of the widget in the viewport or 3D space. |
| **Screen Space Type** | Switch between `World` or `Screen` space rendering. |
| **Change Widget Pitch / Roll / Yaw** | Toggles whether to allow rotation of the widget in each axis. |
| **Show Widget Shadows** | Enables basic dynamic shadow casting on the widget. |
| **Interact Text** | The text shown inside the widget (e.g., "Open", "Pick Up", etc.). |

---

### 🧩 Widget Action Settings

| Property | Description |
|---------|-------------|
| **Widget to Open** | Only used if `Action Type == Widget Action`. Defines the widget to display during interaction (e.g., inventory, dialogue, panel). |

---

