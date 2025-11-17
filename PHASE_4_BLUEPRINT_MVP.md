# Phase 4.0: Blueprint System MVP - Implementation Plan

## 🎯 Overview
Create a simplified visual node-based scripting system (Blueprints) that allows users to create interactive behaviors without writing code. This MVP focuses on the essential features needed for basic game logic.

---

## 📋 MVP Scope (Weeks 1-3)

### **What's Included in MVP:**
- ✅ Visual node canvas with drag-and-drop
- ✅ 8 essential node types (Event, Action, Flow)
- ✅ Visual connection system (wiring)
- ✅ Blueprint execution engine
- ✅ Save/Load blueprints with scene
- ✅ Simple debugging (highlight active nodes)

### **What's NOT in MVP (Future Phases):**
- ❌ Variables and data types
- ❌ Math operations
- ❌ Complex flow control (loops, switches)
- ❌ Custom functions/macros
- ❌ Blueprint debugging tools
- ❌ Multi-object blueprints

---

## 🎨 UI Design

### **Blueprint Editor Location**
Add a new tab in the right panel:
```
┌─────────────────────────────────────┐
│ Properties | Animation | Blueprint  │ ← New Tab
├─────────────────────────────────────┤
│                                     │
│  [Node Palette]                     │
│  ┌───────────────┐                  │
│  │ 📍 Events     │                  │
│  │ ⚡ Actions    │                  │
│  │ 🔀 Flow       │                  │
│  └───────────────┘                  │
│                                     │
│  [Blueprint Canvas]                 │
│  ┌─────────────────────────────┐   │
│  │                             │   │
│  │   Node connections here     │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  [▶️ Test Blueprint] [🗑️ Clear]    │
└─────────────────────────────────────┘
```

### **Node Appearance**
```
┌─────────────────────────┐
│ 📍 On Collision Enter   │ ← Event (Green header)
├─────────────────────────┤
│ ▶ Execute               │ ← Output pin
│ 📦 Other Object         │ ← Data output
└─────────────────────────┘

┌─────────────────────────┐
│ ⚡ Change Color          │ ← Action (Blue header)
├─────────────────────────┤
│ ◀ Execute               │ ← Input pin
│ Color: [#FF0000]        │ ← Parameter
│ ▶ Then                  │ ← Output pin
└─────────────────────────┘

┌─────────────────────────┐
│ 🔀 Branch (If)          │ ← Flow (Yellow header)
├─────────────────────────┤
│ ◀ Execute               │ ← Input pin
│ Condition: [dropdown]   │ ← Parameter
│ ▶ True                  │ ← Output pin (True)
│ ▶ False                 │ ← Output pin (False)
└─────────────────────────┘
```

---

## 🧩 Essential Node Types (MVP)

### **1. Event Nodes** (Green) - Start execution
```javascript
// Node: On Start
{
    type: 'event',
    name: 'On Start',
    icon: '📍',
    outputs: [
        { name: 'Execute', type: 'exec' }
    ],
    execute: (node, context) => {
        // Triggered once when play mode starts
        node.triggerOutput('Execute', context);
    }
}

// Node: On Collision Enter
{
    type: 'event',
    name: 'On Collision Enter',
    icon: '📍',
    outputs: [
        { name: 'Execute', type: 'exec' },
        { name: 'Other Object', type: 'object' }
    ],
    execute: (node, context, otherObject) => {
        context.otherObject = otherObject;
        node.triggerOutput('Execute', context);
    }
}

// Node: On Trigger Enter
// Node: On Click
```

### **2. Action Nodes** (Blue) - Do things
```javascript
// Node: Change Color
{
    type: 'action',
    name: 'Change Color',
    icon: '⚡',
    inputs: [
        { name: 'Execute', type: 'exec' }
    ],
    outputs: [
        { name: 'Then', type: 'exec' }
    ],
    params: [
        { name: 'color', type: 'color', default: '#FF0000' }
    ],
    execute: (node, context) => {
        const obj = context.targetObject;
        if (obj && obj.material) {
            obj.material.color.setHex(parseInt(node.params.color.replace('#', '0x')));
        }
        node.triggerOutput('Then', context);
    }
}

// Node: Move Object
{
    type: 'action',
    name: 'Move Object',
    icon: '⚡',
    params: [
        { name: 'x', type: 'number', default: 0 },
        { name: 'y', type: 'number', default: 1 },
        { name: 'z', type: 'number', default: 0 }
    ],
    execute: (node, context) => {
        const obj = context.targetObject;
        if (obj) {
            obj.position.set(
                parseFloat(node.params.x),
                parseFloat(node.params.y),
                parseFloat(node.params.z)
            );
        }
        node.triggerOutput('Then', context);
    }
}

// Node: Destroy Object
// Node: Play Animation
// Node: Log Message
```

### **3. Flow Control Nodes** (Yellow) - Decide what to do
```javascript
// Node: Branch (If)
{
    type: 'flow',
    name: 'Branch',
    icon: '🔀',
    inputs: [
        { name: 'Execute', type: 'exec' }
    ],
    outputs: [
        { name: 'True', type: 'exec' },
        { name: 'False', type: 'exec' }
    ],
    params: [
        {
            name: 'condition',
            type: 'select',
            options: [
                'Is Player',
                'Has Tag: Enemy',
                'Has Tag: Collectible',
                'Y Position > 0',
                'Y Position < 0'
            ],
            default: 'Is Player'
        }
    ],
    execute: (node, context) => {
        const condition = node.params.condition;
        let result = false;

        // Simple condition evaluation
        const obj = context.otherObject || context.targetObject;
        if (!obj) return;

        switch(condition) {
            case 'Is Player':
                result = obj.name === 'Player' || obj.userData.isPlayer;
                break;
            case 'Has Tag: Enemy':
                result = obj.userData.tag === 'Enemy';
                break;
            case 'Has Tag: Collectible':
                result = obj.userData.tag === 'Collectible';
                break;
            case 'Y Position > 0':
                result = obj.position.y > 0;
                break;
            case 'Y Position < 0':
                result = obj.position.y < 0;
                break;
        }

        node.triggerOutput(result ? 'True' : 'False', context);
    }
}

// Node: Delay (simple setTimeout)
```

---

## 💾 Data Structure

### **Blueprint Storage**
```javascript
object.userData.blueprint = {
    enabled: true,
    nodes: [
        {
            id: 'node_1',
            type: 'event',
            name: 'On Collision Enter',
            position: { x: 50, y: 100 },
            outputs: {
                'Execute': ['node_2.Execute'],
                'Other Object': []
            }
        },
        {
            id: 'node_2',
            type: 'flow',
            name: 'Branch',
            position: { x: 250, y: 100 },
            params: {
                condition: 'Is Player'
            },
            outputs: {
                'True': ['node_3.Execute'],
                'False': []
            }
        },
        {
            id: 'node_3',
            type: 'action',
            name: 'Change Color',
            position: { x: 450, y: 80 },
            params: {
                color: '#00FF00'
            },
            outputs: {
                'Then': []
            }
        }
    ],
    connections: [
        { from: 'node_1.Execute', to: 'node_2.Execute' },
        { from: 'node_2.True', to: 'node_3.Execute' }
    ]
};
```

### **Node Registry (Global)**
```javascript
const blueprintNodeRegistry = {
    events: [
        { name: 'On Start', icon: '📍', definition: {...} },
        { name: 'On Collision Enter', icon: '📍', definition: {...} },
        { name: 'On Trigger Enter', icon: '📍', definition: {...} },
        { name: 'On Click', icon: '📍', definition: {...} }
    ],
    actions: [
        { name: 'Change Color', icon: '⚡', definition: {...} },
        { name: 'Move Object', icon: '⚡', definition: {...} },
        { name: 'Destroy Object', icon: '⚡', definition: {...} },
        { name: 'Log Message', icon: '⚡', definition: {...} }
    ],
    flow: [
        { name: 'Branch', icon: '🔀', definition: {...} },
        { name: 'Delay', icon: '🔀', definition: {...} }
    ]
};
```

---

## 🔧 Implementation Steps

### **Week 1: Canvas & Basic Nodes**
- [ ] Add Blueprint tab to properties panel
- [ ] Create blueprint canvas with zoom/pan
- [ ] Implement node palette (collapsible categories)
- [ ] Create node rendering system
- [ ] Add drag-and-drop node creation
- [ ] Implement node deletion

### **Week 2: Connections & Execution**
- [ ] Visual connection drawing (Bezier curves)
- [ ] Pin connection logic (output → input)
- [ ] Connection validation (type checking)
- [ ] Delete connections (right-click)
- [ ] Build execution engine
- [ ] Integrate with collision/trigger events

### **Week 3: Polish & Integration**
- [ ] Save/Load blueprints with scene JSON
- [ ] Add blueprint enable/disable toggle
- [ ] Node highlighting during execution
- [ ] Error handling and validation
- [ ] Test with example scenes
- [ ] Create blueprint templates

---

## 🎮 Example Blueprint: Collectible Coin

```
┌─────────────────────────┐
│ 📍 On Trigger Enter     │
│ ▶ Execute ──────┐       │
│ 📦 Other ────┐  │       │
└──────────────│──│───────┘
               │  │
               │  └──────────────────┐
               │                     │
               ▼                     ▼
┌─────────────────────────┐  ┌─────────────────────────┐
│ 🔀 Branch               │  │ ⚡ Change Color          │
│ ◀ Execute               │  │ ◀ Execute               │
│ Condition: Is Player    │  │ Color: #FFD700 (Gold)   │
│ ▶ True ─────┐           │  │ ▶ Then                  │
│ ▶ False     │           │  └─────────────────────────┘
└─────────────│───────────┘
              │
              ▼
┌─────────────────────────┐
│ ⚡ Destroy Object        │
│ ◀ Execute               │
│ Target: This            │
└─────────────────────────┘
```

**What it does:**
1. When player enters trigger
2. Check if it's the player (not enemy)
3. If true, turn gold and destroy (collected)
4. If false, do nothing

---

## 🚀 Technical Architecture

### **File Structure (in threejs-pro-editor2.html)**
```javascript
// Blueprint System
const blueprintNodeRegistry = {};  // Node definitions
const blueprintCanvas = {          // Canvas state
    zoom: 1.0,
    pan: { x: 0, y: 0 },
    selectedNode: null,
    dragConnection: null
};

// Core Functions
function initBlueprintSystem()       // Setup node registry
function createBlueprintCanvas()     // Render canvas
function addNodeToPalette()          // Populate node list
function createNode()                // Instantiate node
function drawConnection()            // Render Bezier curve
function executeBlueprint()          // Run blueprint logic
function saveBlueprintToObject()     // Store in userData
function loadBlueprintFromObject()   // Restore from userData
```

### **Event Integration**
```javascript
// In detectCollisions()
if (obj1.userData.blueprint && obj1.userData.blueprint.enabled) {
    const eventNode = obj1.userData.blueprint.nodes.find(n => n.name === 'On Collision Enter');
    if (eventNode) {
        executeBlueprint(obj1, 'On Collision Enter', { otherObject: obj2 });
    }
}

// In play mode start
sceneObjects.forEach(obj => {
    if (obj.userData.blueprint && obj.userData.blueprint.enabled) {
        const eventNode = obj.userData.blueprint.nodes.find(n => n.name === 'On Start');
        if (eventNode) {
            executeBlueprint(obj, 'On Start', {});
        }
    }
});
```

---

## 📊 Success Metrics

### **Performance**
- [ ] 50+ nodes without lag
- [ ] Blueprint execution < 1ms
- [ ] Smooth canvas pan/zoom at 60 FPS

### **Usability**
- [ ] Create a working blueprint in < 2 minutes
- [ ] Visual feedback for all actions
- [ ] Clear error messages for invalid connections

### **Features**
- [ ] 8+ essential nodes working
- [ ] Save/Load blueprints
- [ ] Visual debugging (highlight active nodes)
- [ ] Integration with collision/trigger systems

---

## 🎯 8 Essential Nodes for MVP

### **Events (4 nodes)**
1. **On Start** - Runs once when play mode starts
2. **On Collision Enter** - When physics collision occurs
3. **On Trigger Enter** - When trigger volume entered
4. **On Click** - When object is clicked (raycasting)

### **Actions (4 nodes)**
5. **Change Color** - Modify material color
6. **Move Object** - Set position
7. **Destroy Object** - Remove from scene
8. **Log Message** - Console output for debugging

### **Flow (2 nodes - Bonus)**
9. **Branch (If)** - Conditional execution
10. **Delay** - Wait before next action

---

## 💡 Design Principles

1. **Visual First**: Everything visible on canvas (no hidden logic)
2. **Simple Connections**: One output can connect to multiple inputs
3. **Immediate Feedback**: Highlight nodes as they execute
4. **Error Tolerance**: Invalid connections prevented, not just warned
5. **Template-Driven**: Include example blueprints in templates

---

## 🚧 Known Limitations (MVP)

- No variables or data storage
- No loops (for/while)
- Limited condition checking
- Single-object blueprints only (can't affect other objects directly)
- No custom node creation
- No blueprint reuse across objects

**These will be addressed in Phase 4.1+**

---

## 📝 Implementation Notes

- Use SVG for canvas rendering (easier connections)
- Store connections as adjacency list (from → to)
- Execute blueprints synchronously (simple recursion)
- Use different header colors for node types (Green/Blue/Yellow)
- Add tooltips to all pins explaining their purpose
- Include "Getting Started" blueprint template

---

**Status**: Ready to Implement
**Start Date**: 2025-11-17
**Target Completion**: 3 weeks
**Current Phase**: Architecture Design Complete

Let's build visual scripting! 🎨
