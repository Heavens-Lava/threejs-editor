# Mobile-First Design Guide

## Philosophy

The Three.js Pro Editor is designed with a **mobile-first** approach, ensuring that users can build complete 3D experiences directly from their smartphones or tablets. This document outlines the design principles, UI patterns, and technical considerations for mobile development.

---

## Core Principles

### 1. Touch-First Interactions
All interactions must work primarily through touch, with desktop mouse/keyboard as an enhancement.

### 2. Progressive Enhancement
Features scale up from mobile → tablet → desktop, not down.

### 3. Performance Priority
Mobile devices have limited resources; optimization is critical.

### 4. Context-Aware UI
Interface adapts to screen size, orientation, and device capabilities.

### 5. Accessibility First
Large touch targets, readable text, and clear visual hierarchy.

---

## Responsive Breakpoints

### Mobile Portrait (320px - 480px)
- **Primary Use Case**: Quick edits, blueprint creation, testing
- **Layout**: Single column, full-screen panels
- **Navigation**: Bottom tab bar + hamburger menu
- **Canvas**: 60% of screen, swipe to toggle panels

### Mobile Landscape (481px - 767px)
- **Primary Use Case**: Extended editing, detailed work
- **Layout**: Canvas with overlay panels (collapsible)
- **Navigation**: Side navigation (auto-hide)
- **Canvas**: 70% of screen, split-view option

### Tablet Portrait (768px - 1024px)
- **Primary Use Case**: Professional mobile work
- **Layout**: Two-column layout possible
- **Navigation**: Persistent sidebar (narrow)
- **Canvas**: 65% of screen, with side panels

### Tablet Landscape (1025px - 1366px)
- **Primary Use Case**: Full editing experience
- **Layout**: Desktop-like with adaptations
- **Navigation**: Full sidebar navigation
- **Canvas**: 50% of screen, balanced layout

### Desktop (1367px+)
- **Primary Use Case**: Power user workflow
- **Layout**: Full three-panel layout
- **Navigation**: All panels visible simultaneously
- **Canvas**: Centered with side panels

---

## Mobile UI Components

### 1. Bottom Navigation Bar (Mobile)

```
┌─────────────────────────────────────┐
│                                     │
│         CANVAS AREA                 │
│                                     │
│                                     │
├─────────────────────────────────────┤
│ [Assets] [Tools] [Play] [Props] [≡] │
└─────────────────────────────────────┘
```

**Features:**
- Fixed position at bottom (thumb-friendly)
- 5 main sections with icons
- Active state highlighting
- Swipe up for quick access panel
- Haptic feedback on tap

**Touch Targets:**
- Minimum 44px height
- Minimum 44px width
- 8px spacing between items

### 2. Expandable Panels

**Behavior:**
- Swipe up from bottom nav to expand panel
- Swipe down to collapse
- Drag handle at top for easy grabbing
- Three states: collapsed (0%), half (50%), full (90%)
- Backdrop blur when expanded

**Panel Types:**
- Asset Library Panel
- Properties Panel
- Blueprint Editor Panel
- Scene Hierarchy Panel
- Settings Panel

### 3. Floating Action Button (FAB)

**Primary Actions:**
- Add Object (tap to open quick menu)
- Create Blueprint
- Play/Pause
- Undo/Redo (long press for history)

**Position:**
- Bottom right (above navigation bar)
- Expandable to show related actions
- Draggable to preferred position

### 4. Context-Sensitive Toolbar

**Appears when object selected:**
- Transform tools (move, rotate, scale)
- Quick property adjustments
- Delete, duplicate, copy
- Lock/unlock, show/hide

**Position:**
- Floating above selected object
- Or in top bar if object not visible

### 5. Touch Gestures

#### Canvas Gestures
- **Single Tap**: Select object
- **Double Tap**: Focus on object
- **Long Press**: Context menu
- **Pinch**: Zoom in/out
- **Two-Finger Drag**: Pan camera
- **Three-Finger Drag**: Rotate camera
- **Two-Finger Rotate**: Rotate camera around up axis
- **Swipe from Edge**: Open/close panels

#### Blueprint Editor Gestures
- **Tap**: Select node
- **Double Tap**: Edit node
- **Drag**: Move node
- **Pinch**: Zoom canvas
- **Two-Finger Drag**: Pan canvas
- **Long Press**: Node options menu
- **Drag from Port**: Create connection

### 6. Mobile-Optimized Transform Controls

**Widget Design:**
- Larger touch targets (3x desktop size)
- Color-coded axes (Red=X, Green=Y, Blue=Z)
- Simplified handles (fewer but larger)
- Visual feedback on touch (glow, scale up)
- Haptic feedback on snap points

**Modes:**
- **Move**: Single-axis arrows + free move (center)
- **Rotate**: Axis-aligned rings + free rotate
- **Scale**: Corner handles + uniform scale (center)

**Quick Mode Switch:**
- Swipe on widget to cycle modes
- Tap widget center for mode menu

### 7. Virtual Joystick (Play Mode)

**Position:** Bottom left
**Features:**
- Appears only in play mode
- Semi-transparent
- Dynamic (appears where finger touches)
- Customizable size and sensitivity
- Visual feedback of direction

**Additional Controls:**
- Action buttons (right side)
- Jump, interact, shoot, etc.
- Customizable layout

---

## Mobile-Specific Features

### 1. Quick Actions Menu

**Access:** Long press on canvas empty space

**Actions:**
- Add Object (with preview)
- Create Light
- Add Camera
- Apply Blueprint
- Scene Settings
- Help

### 2. Property Quick Edit

**Inline Editors:**
- Slider for numeric values (position, rotation, scale)
- Color wheel for colors (large, thumb-friendly)
- Toggle switches instead of checkboxes
- Dropdown with large tap targets
- Stepper buttons for incremental changes

### 3. Blueprint Creator Wizard (Mobile)

**Step-by-Step Interface:**
1. What do you want to create? (Icons + text)
2. Choose base template (Visual previews)
3. Customize parameters (Simple sliders)
4. Name and save

**Features:**
- Large, tappable cards
- Visual previews of each option
- "Skip" to use defaults
- "More options" for advanced users
- Swipe between steps

### 4. Smart Suggestions

**Context-Aware Prompts:**
- "Add a floor to your scene?"
- "This object needs a collision shape"
- "Make this interactive?"

**Position:** Bottom sheet notification
**Interaction:** Tap to apply, swipe to dismiss

### 5. Voice Commands (Future)

**Basic Commands:**
- "Add a cube"
- "Move object up"
- "Change color to red"
- "Play scene"
- "Create blueprint for jumping"

### 6. Simplified Mode Toggle

**Beginner Mode:**
- Hide advanced options
- Guided workflows
- Tooltips always visible
- Limited but essential features

**Advanced Mode:**
- All features available
- Customizable interface
- Keyboard shortcuts (if keyboard attached)
- Power user tools

---

## Layout Patterns

### Pattern 1: Full-Screen Canvas (Mobile Portrait)

```
┌─────────────────┐
│  Hamburger  [?] │ <- Top bar (thin)
├─────────────────┤
│                 │
│                 │
│     CANVAS      │
│                 │
│                 │
├─────────────────┤
│ Bottom Nav Bar  │ <- Fixed bottom nav
└─────────────────┘
```

### Pattern 2: Split View (Mobile Landscape)

```
┌─────────────────────────────────┐
│ Properties  │      CANVAS       │
│   Panel     │                   │
│  (30%)      │      (70%)        │
│             │                   │
└─────────────────────────────────┘
   Bottom Nav Bar (collapsible)
```

### Pattern 3: Overlay Panel (Any Mobile)

```
┌─────────────────┐
│                 │
│     CANVAS      │ <- Darkened when panel open
│                 │
├─────────────────┤
│ ▔▔▔▔▔▔▔▔▔      │ <- Drag handle
│ Panel Title  [X]│
│                 │
│ Panel Content   │
│                 │
└─────────────────┘
```

### Pattern 4: Tablet Two-Panel

```
┌──────────────────────────────────────┐
│ Toolbar                              │
├─────────┬──────────────────┬─────────┤
│ Assets  │                  │ Props   │
│ (20%)   │    CANVAS (60%)  │ (20%)   │
│         │                  │         │
│         │                  │         │
└─────────┴──────────────────┴─────────┘
```

---

## Touch-Optimized Controls

### 1. Sliders
- **Height**: 44px minimum
- **Thumb Size**: 32px
- **Track**: 8px thick, clear visual feedback
- **Snap Points**: Visible markers, haptic feedback
- **Value Display**: Large, above thumb

### 2. Buttons
- **Minimum Size**: 44x44px
- **Padding**: 12px internal
- **Font Size**: 16px minimum
- **Icon Size**: 24px minimum
- **Spacing**: 8px between buttons
- **States**: Clear visual feedback (tap, hold, active)

### 3. Input Fields
- **Height**: 48px minimum
- **Font Size**: 16px (prevents zoom on iOS)
- **Padding**: 12px
- **Clear Button**: Large X to clear text
- **Helper Text**: Below field, not as placeholder

### 4. Dropdowns
- **Trigger**: 44px minimum height
- **Options**: 48px height per option
- **Visual**: Large checkmark for selected
- **Search**: Auto-focus search for long lists
- **Native Picker**: Use native pickers when appropriate

### 5. Color Picker
- **Size**: Large circle picker (200px diameter)
- **Sliders**: Large sliders for HSV/RGB
- **Presets**: Large color swatches (48x48px)
- **Hex Input**: Large text field
- **Recent Colors**: Quick access palette

### 6. Vector Inputs (X, Y, Z)
- **Layout**: Vertical stack on small screens
- **Steppers**: +/- buttons on sides
- **Drag to Change**: Drag on label to adjust value
- **Visual Gizmo**: 3D widget alternative

---

## Performance Optimization for Mobile

### 1. Rendering Optimizations
- **Target FPS**: 60fps on flagship, 30fps on budget devices
- **Auto-Quality**: Detect device capabilities and adjust
- **LOD System**: Aggressive level-of-detail switching
- **Draw Calls**: Batch and instance aggressively
- **Shadow Quality**: Reduce on mobile, or disable
- **Post-Processing**: Simplified or disabled on low-end

### 2. Memory Management
- **Texture Compression**: ETC2, ASTC for mobile
- **Texture Size**: Max 2048x2048 on mobile
- **Mesh Optimization**: Reduce poly count
- **Object Pooling**: Reuse objects instead of create/destroy
- **Lazy Loading**: Load assets on demand
- **Unload Unused**: Aggressive garbage collection

### 3. Loading Strategies
- **Progressive Loading**: Show UI immediately, load assets progressively
- **Skeleton Screens**: Show structure while loading
- **Priority Loading**: UI first, then visible assets, then hidden
- **Background Loading**: Load next scene in background
- **Caching**: Aggressive caching of frequently used assets

### 4. Network Optimization
- **Offline First**: Work offline, sync when online
- **Reduced Payloads**: Compress all network data
- **CDN**: Use CDN for asset delivery
- **Service Workers**: Cache assets locally

---

## Mobile-Specific Blueprint Features

### 1. Tap-Based Blueprint Creation

**Interface:**
- Large category cards (Game, Web, Effects, etc.)
- Tap to see subcategories
- Tap to see blueprints
- Preview animation before adding
- One-tap to add to scene

### 2. Question-Based Builder

**Flow:**
```
Q: What do you want to create?
→ [A Game] [A Website] [A Visualization]

Q: What type of game?
→ [Platformer] [FPS] [Puzzle] [Racing]

Q: How should the character move?
→ [Arrow Keys] [WASD] [Touch Joystick] [Swipe]

Q: Should the character be able to jump?
→ [Yes, single jump] [Yes, double jump] [No]

✓ Blueprint Created! Tap to customize or add to scene.
```

**Features:**
- Large tappable options
- Visual icons for each choice
- "Go Back" to change answers
- "Skip" to use defaults
- Preview updates as you answer

### 3. Voice-Activated Blueprint

**Example Commands:**
- "Create a character that can jump and run"
- "Add a first-person controller"
- "Make this object glow when I click it"
- "Add a game timer that counts down from 60 seconds"

**Feedback:**
- Show blueprint being created in real-time
- Confirm action before applying
- Suggest refinements

---

## Accessibility Considerations

### 1. Visual
- **Contrast Ratios**: WCAG AAA compliant (7:1)
- **Font Size**: Minimum 16px, scalable
- **Color Coding**: Never rely solely on color
- **Icons**: Always paired with labels (or accessible labels)
- **Dark/Light Themes**: Both available

### 2. Motor
- **Large Touch Targets**: 44x44px minimum
- **No Precise Gestures**: Avoid complex multi-finger gestures
- **Generous Timing**: No time-critical interactions
- **Undo Always Available**: Forgiving interactions
- **Voice Control**: Alternative to touch

### 3. Cognitive
- **Simple Language**: Clear, concise labels
- **Consistent Patterns**: Same interaction patterns throughout
- **Visual Hierarchy**: Clear importance indicators
- **Progressive Disclosure**: Don't overwhelm with options
- **Help Always Visible**: ? icon always accessible

### 4. Connectivity
- **Offline Support**: Core features work offline
- **Low Bandwidth Mode**: Reduce asset quality
- **Progress Indicators**: Show loading/saving status
- **Graceful Degradation**: Work with poor connection

---

## Testing Requirements

### Device Testing Matrix

**Must Test:**
- iPhone SE (small screen, iOS)
- iPhone 14 Pro (modern iOS)
- Samsung Galaxy S22 (modern Android)
- iPad Air (tablet)
- Budget Android (low-end device)

**Browsers:**
- Safari (iOS)
- Chrome (Android)
- Firefox (Android)
- Samsung Internet

**Orientations:**
- Portrait
- Landscape
- Rotation handling

### Performance Targets

| Device Tier | FPS | Load Time | Memory |
|------------|-----|-----------|--------|
| High-end | 60 | < 2s | < 200MB |
| Mid-range | 45 | < 3s | < 150MB |
| Low-end | 30 | < 5s | < 100MB |

### User Testing Scenarios

1. **New User**: Can they create a simple scene in under 5 minutes?
2. **Blueprint Creation**: Can they find and apply blueprints easily?
3. **Mobile Editing**: Can they transform objects precisely?
4. **Export**: Can they export and share their creation?
5. **Play Mode**: Can they test their scene on mobile?

---

## Progressive Web App (PWA) Features

### 1. Installable
- Add to home screen prompt
- Standalone mode (no browser UI)
- Custom splash screen
- App icon

### 2. Offline Capable
- Service worker for caching
- IndexedDB for local projects
- Background sync for cloud saves

### 3. Push Notifications
- Project auto-saved
- Collaboration updates
- New template available
- Tutorial completion

### 4. Performance
- App shell architecture
- Route-based code splitting
- Lazy loading of features
- Precaching of critical assets

---

## Mobile UI Component Library

### Components to Build

1. **MobileBottomNav**: Main navigation bar
2. **ExpandablePanel**: Swipeable panels
3. **TouchTransformControls**: 3D gizmo for mobile
4. **MobilePropEditor**: Property editing interface
5. **TouchJoystick**: Virtual joystick for play mode
6. **MobileColorPicker**: Touch-optimized color picker
7. **MobileVectorInput**: X,Y,Z input for mobile
8. **MobileBlueprintWizard**: Step-by-step blueprint creator
9. **MobileContextMenu**: Long-press context menu
10. **MobileToast**: Non-intrusive notifications
11. **MobileModal**: Full-screen modals for mobile
12. **MobileAccordion**: Collapsible sections
13. **MobileTabs**: Touch-optimized tabs
14. **MobileSlider**: Large, touch-friendly slider
15. **MobileToggle**: Switch/toggle component

---

## Implementation Checklist

### Phase 1: Foundation
- [ ] Responsive breakpoints system
- [ ] Touch event handling
- [ ] Gesture recognition
- [ ] Mobile-first CSS structure
- [ ] Performance monitoring

### Phase 2: Core UI
- [ ] Bottom navigation bar
- [ ] Expandable panel system
- [ ] Mobile transform controls
- [ ] Touch-optimized property editors
- [ ] FAB (Floating Action Button)

### Phase 3: Advanced Features
- [ ] Blueprint wizard UI
- [ ] Virtual joystick
- [ ] Voice command framework
- [ ] Offline support
- [ ] PWA configuration

### Phase 4: Optimization
- [ ] Asset compression
- [ ] Code splitting
- [ ] Lazy loading
- [ ] Service worker setup
- [ ] Performance profiling

### Phase 5: Polish
- [ ] Animations and transitions
- [ ] Haptic feedback
- [ ] Sound effects
- [ ] Accessibility audit
- [ ] User testing

---

## Best Practices

### DO:
✅ Design for thumb reach zones
✅ Provide immediate visual feedback
✅ Use native mobile patterns when possible
✅ Test on real devices early and often
✅ Optimize for battery life
✅ Support landscape and portrait
✅ Make text selectable and copyable
✅ Provide undo for all destructive actions

### DON'T:
❌ Rely on hover states
❌ Use tooltips that appear on hover
❌ Make touch targets smaller than 44px
❌ Use complex multi-step gestures
❌ Assume high-speed connection
❌ Force portrait or landscape only
❌ Auto-play videos or sounds
❌ Show too much information at once

---

## Resources & Tools

### Design Tools
- **Figma**: For mobile UI mockups
- **Principle**: For animation prototypes
- **Marvel**: For mobile prototypes

### Testing Tools
- **Chrome DevTools**: Device emulation
- **BrowserStack**: Real device testing
- **WebPageTest**: Performance testing
- **Lighthouse**: Mobile audits

### Libraries
- **Hammer.js**: Touch gesture library
- **React Spring**: Animations
- **Nipple.js**: Virtual joystick
- **Workbox**: Service worker library

---

**Document Version**: 1.0
**Last Updated**: 2025-11-17
**Status**: Design Guide - Implementation Ready
