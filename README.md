# 📦 Modular Interactions Plugin
A highly customizable interaction system built for Unreal Engine 5. This plugin provides developers with a modular, extensible foundation for adding interaction logic to actors, complete with dynamic widgets, reusable behavior types, and clean C++/Blueprint integration.


-[DISCORD SUPPORT LINK HERE](https://discord.gg/YvME6jG8)

-[FAB LINK HERE]()

## 📑 Table of Contents
- [🚀 Getting Started](#-getting-started)
- [⚙️ Interactable Base – Settings Reference](#️-interactable-base--settings-reference-with-conditions)
- [🎬 Animation](#-animation)
- [🔁 Relay Actor Settings](#-interaction-relay-actor--settings-reference-with-conditions)
- [📦 Creating Custom Interactable Actors](#-creating-custom-interactable-actors)
- [📸 Showcase / Previews](#-showcase--previews)



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

## 1. Enable Plugin

Go to `Edit > Plugins` and enable the **Modular Interactions** plugin.

## 2. Add the Interaction Handler to Your Character

Add the `BP_InteractionHandlerComponent` to **your main character**.  
This component is required and handles most of the core interaction logic. It must be added directly to the player’s character so the plugin can consistently reference it during interaction.

Once the component is added, you're ready to begin setting up interactables:

- Use `AInteractableActorBase` to define objects that can be interacted with.
- Use `AInteractionRelayActor` to control one or more interactables remotely — great for switches, buttons, and other external triggers.

📌 *See the image below for a reference of available classes.*
IE: `InteractableActorBase`, `InteractionRelayActor`

![InteractBPVisual](https://github.com/user-attachments/assets/1839f81b-7fc2-4081-ae07-531d29983d12)

## 3. Configure Interaction Settings

Each interactable actor includes a modular, dynamic details panel for setup:

- **Widget Type**: Choose from Tooltip, Icon Prompt, Radial Progress, or Custom.
- **Action Type**: Choose from Widget Action, Toggle Actor, Custom Event, and more.

🔄 The panel dynamically updates based on your selections:

- Enabling **Use Built-in Action** exposes pre-wired logic like widget toggling.
- Selecting different **Widget Types** reveals widget-specific options.
- Enabling **Use Animation** displays animation-specific settings.

This system keeps clutter to a minimum and only shows settings that are relevant to your current configuration.

## 4. Relay Interactable Actors (Optional)

Relay actors (`AInteractionRelayActor`) are designed to **remotely trigger other interactables**.  
These are ideal for buttons, control panels, switches, and similar use cases.

Relay-specific settings allow you to:

- Link multiple target actors
- Automatically follow or attach to another actor
- Control how it aligns and snaps in the world

📌 *See the image below for an example of relay actor configuration.*

![InteractionRelaySettings](https://github.com/user-attachments/assets/20d4fc19-c5cd-410a-8a11-f033c7e7ea95)


## 5. Hook Up Inputs

Make sure your character has the correct input mappings via **Enhanced Input**.  
Alternatively, you can directly call `FinalInteract()` in custom input setups.

---

## ⚙️ Interactable Base – Settings Reference (With Conditions)

These settings are available when working with `AInteractableActorBase`. The panel is dynamic and changes based on selected enum options like `Action Type` and `Widget Type`.



| Property | Type | Description | Visibility Condition |
|---------|------|-------------|----------------------|
| `bUseBuiltInAction` | `bool` | Enables built-in behavior (e.g., toggle actor, show widget). Disabling allows you to fully override interaction via `OnInteract()`. | Always visible *(if `bShowCoreSettings` is true)* |
| `ActionType` | `EActionType` | Defines which behavior to run when interacted with. Options include `WidgetAction`, `ToggleActor`, `TimelineMove`, etc. | Visible only if `bUseBuiltInAction` **and** `bShowCoreSettings` are true |
| `bUseAnimation` | `bool` | Enables animation support. Additional animation-related settings appear when this is true. | Always visible |
| `AnimationToPlay` | `UAnimMontage*` | Animation to play when `bUseAnimation` is enabled. | Only shown if `bUseAnimation` is true |
| `bCanMoveDuringMontage` | `bool` | Allows player to move during animation playback. | Only shown if `bUseAnimation` is true |
| `bFullBodyAnim` | `bool` | Treats the animation montage as full-body. | Only shown if `bUseAnimation` is true |
| `bControlledByOtherInteractable` | `bool` | Prevents this actor from handling its own interaction. Typically used when another actor (like a relay) is in control. | Always visible *(if `bShowCoreSettings` is true)* |
| `bRequiresManualTrigger` | `bool` | Requires the actor to be triggered by external input or systems (e.g., interaction relay). | Always visible |
| `WidgetType` | `EWidgetType` | Selects the visual interaction widget (e.g., radial, tooltip, custom). | Hidden if `bControlledByOtherInteractable` is true |
| `CustomWidgetClass` | `TSubclassOf<UUserWidget>` | Assign a UMG class to use when `WidgetType == CustomWidget`. | Only visible when `WidgetType == CustomWidget` |
| `WidgetOffset` | `FVector` | Adjusts the position of the widget relative to the actor. | Hidden if `bControlledByOtherInteractable` is true |
| `WidgetScale` | `FVector` | Controls the size of the widget in 3D. | Only visible if `ScreenSpaceType == World` and `bControlledByOtherInteractable == false` |
| `ScreenSpaceType` | `EWidgetSpace` | Toggles between world space and screen space rendering for widgets. | Hidden if `bControlledByOtherInteractable` is true |
| `bChangeWidgetPitch` | `bool` | Allows the widget to rotate in pitch to face the player. | Visible only if `ScreenSpaceType == World` and not controlled by another actor |
| `bChangeWidgetRoll` | `bool` | Allows the widget to rotate in roll to face the player. | Same as above |
| `bChangeWidgetYaw` | `bool` | Allows the widget to rotate in yaw to face the player. | Same as above |
| `bShowWidgetShadows` | `bool` | Toggles basic dynamic shadows on the widget. | Hidden if `bControlledByOtherInteractable` is true |
| `InteractText` | `FString` | Text displayed in radial progress widgets. | Only visible when `WidgetType == RadialProgress` and not relay-controlled |
| `InteractToolTip` | `FString` | Text shown in tooltip widgets. | Only visible when `WidgetType == ContextToolTip` and not relay-controlled |
| `Icon` | `UTexture2D*` | Icon used in icon-based widgets. | Only visible when `WidgetType == IconBasedPrompt` and not relay-controlled |
| `WidgetToOpen` | `TSubclassOf<UUserWidget>` | Widget to open when `ActionType == WidgetAction`. Ignored if using custom widget. | Only visible if `ActionType == WidgetAction` and `bUseBuiltInAction == true` and `WidgetType != CustomWidget` |
| `TargetActorToToggle` | `AActor*` | Actor to toggle visibility/state when `ActionType == ToggleActor`. | Only visible if `ActionType == ToggleActor` and `bUseBuiltInAction == true` |

---

### 🎬 Animation

Unlike traditional systems that automatically handle montages, the Modular Interactions Plugin gives **full animation control to the developer**.  
The plugin provides basic configuration fields and overrideable events so you can wire up animation however you'd like.

| Property | Description |
|---------|-------------|
| `Use Animation` | Enables the animation system for this interactable. Does not auto-play animations—it unlocks the properties below. |
| `AnimationToPlay` | Developer-assigned `UAnimMontage` reference. You are responsible for manually triggering this montage. |
| `bCanMoveDuringMontage` | Optional flag you can reference in your own logic to allow player movement during interaction. |
| `bFullBodyAnim` | Optional flag for indicating full-body playback—purely informational, you must implement how this affects playback.

To use animations, implement the following **overrideable interface events**:

| Event | Purpose |
|-------|---------|
| `Play Montage Request` | Called when interaction starts. Use this to play your `AnimationToPlay` montage. |
| `Montage Playing` | Fires when the montage begins. |
| `Montage Completed` | Fires when the montage ends. Use this to finalize interaction behavior (e.g., opening a door after animation completes).

📌 *These events must be implemented manually in Blueprint or C++. No animation is triggered by default.*

![MontageOverideEvents](https://github.com/user-attachments/assets/d25a482c-cff1-4627-9bb4-eb4b1ffe6b98)

---

## 🔁 Interaction Relay Actor – Settings Reference (With Conditions)

Relay actors inherit from `AInteractableActorBase` but are used to **trigger other interactables**, not perform interaction logic themselves. As such, some base class settings (e.g., animation, toggle actor) are **automatically disabled or ignored**.

### 🔗 Relay-Specific Settings

| Property | Type | Description | Visibility Condition |
|----------|------|-------------|----------------------|
| `TargetActors` | `TArray<AInteractableActorBase*>` | List of interactable actors this relay will trigger when used. | Always visible |
| `bTriggerAllTargets` | `bool` | When true, all actors in the `TargetActors` list will be triggered. When false, only one is triggered by index. | Always visible |
| `TargetIndexToTrigger` | `int32` | The index of the actor to trigger (used when `bTriggerAllTargets` is false). | Only visible if `bTriggerAllTargets == false` |
| `InteractableToAttachTo` | `AInteractableActorBase*` | If set, this relay will follow and optionally attach to the target actor. | Always visible |
| `AttachmentMode` | `ESimpleAttachmentMode` | Defines how this relay should attach to the specified target (`SnapAll`, `KeepWorldAll`, `KeepRelativeAll`). | Always visible |

### ⚠️ Base Class Settings Ignored or Overridden by Relay Actors

While `AInteractionRelayActor` inherits from `AInteractableActorBase`, certain settings are internally overridden or locked to avoid conflicts with the relay’s purpose (i.e., triggering other actors).

| Ignored or Locked Setting | Category | Reason |
|---------------------------|----------|--------|
| `bUseBuiltInAction` | Main Settings | Relay actors override interaction logic to relay behavior, not perform their own. This setting is disabled. |
| `ActionType` | Main Settings | Relay actors do not execute their own action types; instead they forward interaction to others. |
| `bControlledByOtherInteractable` | Main Settings | Relays are intended to **control** other actors, not be controlled themselves. This is forcibly disabled. |

✅ **Allowed Settings**:
- `WidgetType`, `CustomWidgetClass`, and all other **Widget Settings** are supported. You can show tooltips, icons, prompts, etc.
- All **Animation Settings** (`bUseAnimation`, `AnimationToPlay`, `bFullBodyAnim`) are still available. Relays can play animations like button presses before triggering other actors.

💡 *Tip: Use visual widgets and animations to represent buttons, switches, levers, and other trigger points—but let the interaction logic be forwarded to the real targets.*


---
## 📦 Creating Custom Interactable Actors

The Modular Interactions Plugin is designed to support full Blueprint-based extension. You can create custom interactable actors without touching C++, and still take full advantage of widget prompts, animations, and plugin-driven interaction behavior.

---

## 🔨 Step 1: Create a New Blueprint

1. In the Content Browser, **right-click** and choose **Blueprint Class**
2. In the **All Classes** search box, type `InteractableActorBase`
3. Select `Interactable Actor Base` – this is the base class provided by the plugin
4. Name your Blueprint something like `BPIA_Terminal`, `BPIA_Lever`, or any name using the optional `BPIA_` prefix to indicate it's a Blueprint Interactable Actor

> 📛 **Naming Tip:** Use the `BPIA_` prefix to stay organized when working with multiple custom interactables.
> 
![image 25](https://github.com/user-attachments/assets/e5835cff-f5d2-401f-8db3-2e8e0b253f19)
![image 26](https://github.com/user-attachments/assets/4e9ad2fe-be44-4e80-9c32-386aec9ad742)

## 🧩 Step 2: Add Custom Interaction Logic

1. Inside your Blueprint, open the **Functions** section
2. Click **Override** and select `Interact`
3. Add your custom logic in the graph – this will execute when the player interacts with the actor

![image 27](https://github.com/user-attachments/assets/7d601e54-1a83-45ea-bc66-f624bb546d2e)

> 💡 `Interact` is the core entry point for custom behavior. It is automatically called when interaction is triggered.

## ⚙️ Step 3: Configure Behavior in the Details Panel

Customize your Blueprint instance in the Details Panel:

- **Widget Type** – Choose from Radial, Tooltip, Icon, or Custom
- **Action Type** – Set to `CustomEvent` to disable built-in logic
- **Interact Text** – Customize what appears in radial or tooltip widgets
- **Custom Widget Class** – Assign your own UMG class if using `CustomWidget`

Optional:
- Enable `bUseAnimation` to play a montage on interact
- Use `TargetActorToToggle` if you want to toggle another actor’s state

> ⚠️ `PreInteract` is reserved for internal plugin use and **cannot** be overridden in Blueprints.


### 🖥 Advanced (Optional)

You **can** override `ShowInteractWidget` and `HideInteractWidget` if you want to add visual effects or logic when the widget appears or disappears. However, the built-in system handles this automatically and usually requires no changes.


## 🧪 Step 4: Place and Test

Drag your custom Blueprint into the level and press Play. Interact using your configured input key and verify your logic triggers correctly.

> 🧠 **Tip:** For more complex systems, consider using relay actors to chain multiple interactables or trigger remote behaviors.
---

## 📸 Showcase / Previews

## Below are visual examples of how the Modular Interactions Plugin looks in action and how different settings affect the behavior of interactables and relays.

### 🎯 Widget Types Preview

Here's how each widget type appears when activated:

![image 22](https://github.com/user-attachments/assets/1c83bcff-89ca-4462-93a4-bd798994a260)
![image 23](https://github.com/user-attachments/assets/94e803f3-df83-434d-9176-2dff329fbe8b)

---

### 🧱 Relay Actor in Action

A button actor triggering a door via relay system:

![image 24](https://github.com/user-attachments/assets/04fb321a-f8d3-4c96-929a-85404d7d18f1)


---

