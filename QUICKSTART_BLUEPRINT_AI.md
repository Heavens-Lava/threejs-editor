# Quick Start Guide: Blueprint AI Assistant

## 🚀 How to Use the Blueprint AI Assistant

### Step 1: Refresh Your Browser
After updating the code, make sure to refresh:
- Press `F5` or `Ctrl+R` (Windows/Linux)
- Press `Cmd+R` (Mac)

### Step 2: Create or Select an Object
- Click "**+ Add Object**" button in the top toolbar, OR
- Click on an existing object in the 3D viewport

### Step 3: Navigate to Blueprint Panel
- Look at the **right sidebar**
- Click the "**Blueprint**" tab (if not already selected)

### Step 4: Enable Blueprint
- You'll see a checkbox: "**Enable Blueprint**"
- Check this box

### Step 5: Click the AI Assistant Button
You should now see a prominent purple gradient button:

```
┌─────────────────────────────────────────┐
│  🪄  Create with AI Assistant            │
│      Build blueprints by answering      │
│      questions                           │
└─────────────────────────────────────────┘
```

Click this button!

### Step 6: Answer Questions
1. **Welcome Screen** appears with features
   - Click "Get Started →"

2. **Question 1**: "What would you like to create?"
   - Choose: Game Mechanics, Visual Effects, Interactions, or Simple Behavior

3. **Question 2**: More specific options based on your choice
   - Example: If you chose "Interactions" → Choose "Click Action"

4. **Question 3**: Final details
   - Example: "What happens on click?" → Choose "Change Color"

### Step 7: Blueprint Auto-Generated!
- The system creates all the nodes automatically
- Connections are wired up correctly
- You'll see a success message
- Your blueprint is ready to test!

---

## 🎨 What You Should See

### Location in UI:
```
┌────────────────────────────────────────────┐
│  [Toolbar with Add Object, Play, etc.]    │
├──────┬─────────────────────────┬───────────┤
│      │                         │           │
│ Left │   3D Viewport           │  Right    │
│ Side │                         │  Sidebar  │
│ bar  │                         │           │
│      │                         │ ┌───────┐ │
│      │                         │ │ Props │ │
│      │                         │ │BLUEPRINT│ <- Click here
│      │                         │ │Physics│ │
│      │                         │ └───────┘ │
│      │                         │           │
│      │                         │ [✓] Enable│
│      │                         │  Blueprint│
│      │                         │           │
│      │                         │ ┌───────┐ │
│      │                         │ │  🪄   │ │<- AI Button!
│      │                         │ │Create │ │
│      │                         │ │with AI│ │
│      │                         │ └───────┘ │
└──────┴─────────────────────────┴───────────┘
```

---

## 🔍 Troubleshooting

### Problem: I don't see the AI Assistant button

**Solution 1: Did you refresh?**
- Hard refresh: `Ctrl+Shift+R` or `Cmd+Shift+R`

**Solution 2: Did you select an object?**
- Create a cube or sphere first
- Click on it to select it

**Solution 3: Did you enable Blueprint?**
- Check the "Enable Blueprint" checkbox

**Solution 4: Check the right sidebar**
- Make sure you're on the "Blueprint" tab, not "Properties" or "Physics"

---

### Problem: Button is there but nothing happens when I click

**Check console for errors:**
- Press `F12` to open developer tools
- Look for any red error messages
- Share them if you see any

---

## 📸 Expected Visual Experience

### The AI Assistant Button:
- **Color**: Purple gradient (light to dark purple)
- **Icon**: Magic wand emoji 🪄
- **Text**: "Create with AI Assistant"
- **Size**: Full width of the blueprint panel
- **Effect**: Shimmers on hover, glows slightly

### The Modal Dialog:
- **Appearance**: Large centered dialog (800px wide)
- **Header**: Purple gradient with "Blueprint AI Assistant"
- **Progress Bar**: Shows your current step (e.g., "Step 1 of 4")
- **Content**: Welcome screen or question cards
- **Navigation**: Back and Next buttons at bottom

---

## ✨ Example Usage Scenarios

### Scenario 1: Make Object Change Color When Clicked
1. Select object
2. Enable Blueprint
3. Click AI Assistant
4. Choose: "Interactions" → "Click Action" → "Change Color"
5. Done! Blueprint created.

### Scenario 2: Make Object Jump When Clicked
1. Select object
2. Enable Blueprint
3. Click AI Assistant
4. Choose: "Game Mechanics" → "Movement" → "Jump Upward"
5. Done! Blueprint created.

### Scenario 3: Make Object Disappear After 3 Seconds
1. Select object
2. Enable Blueprint
3. Click AI Assistant
4. Choose: "Simple Behavior" → "Auto Destroy"
5. Done! Blueprint created with delay node.

---

## 🎯 Current Templates Available

Phase 1 includes these auto-generated blueprints:

1. **Click → Change Color** (Changes to red when clicked)
2. **Click → Destroy** (Removes object when clicked)
3. **Click → Log Message** (Console message when clicked)
4. **Click → Jump** (Moves up when clicked)
5. **Auto Destroy** (Disappears after 3 seconds)
6. **Constant Rotation** (Note shown - full implementation coming)
7. **Start → Change Color** (Changes color when play starts)

More templates coming in Phase 2!

---

## 💡 Tips

- **Test in Play Mode**: After creating a blueprint, click "Play" to test it
- **Modify After**: You can edit the auto-generated nodes manually
- **Experiment**: Try different question paths to see various blueprints
- **Save Your Work**: Blueprints are saved with the scene automatically

---

## 🆘 Need Help?

If you're still having issues:
1. Check that `threejs-pro-editor2.html` is the file you're opening
2. Verify the file was saved after the update
3. Try a hard refresh or clear browser cache
4. Check browser console (F12) for error messages

---

**Last Updated**: 2025-11-17
**Phase**: 1 - Foundation Complete
**Status**: ✅ Fully Functional
