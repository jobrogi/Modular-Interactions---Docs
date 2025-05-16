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

| Property | Description |
|---------|-------------|
| **Use Animation** | Enables animation-related settings (e.g., montage playback). More settings will appear once this is enabled. |

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

### ⚠️ Base Class Settings Ignored by Relay Actors

These inherited settings are **not used** in the context of relay actors and will typically have no effect:

| Ignored Setting | Reason |
|------------------|--------|
| `TargetActorToToggle` | Relay does not toggle itself or its own targets via this property. |
| `AnimationToPlay` | Relay actors do not play animations on interaction. |
| `bUseAnimation` / `bCanMoveDuringMontage` / `bFullBodyAnim` | All animation settings are irrelevant for relay actors. |
| `WidgetToOpen` | Relay does not open its own widgets via `WidgetAction`. It is meant to pass interaction logic to others. |

💡 *Tip: You can still use `WidgetType` and other UI properties to give the relay a visible prompt (e.g., a button icon or tooltip), but its logic should focus on forwarding interaction to other actors.*


---

### 🧩 Widget Action Settings

| Property | Description |
|---------|-------------|
| **Widget to Open** | Only used if `Action Type == Widget Action`. Defines the widget to display during interaction (e.g., inventory, dialogue, panel). |

---

