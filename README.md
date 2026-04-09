# telegram-colored-inlinebuttons-premium-emoji
```markdown
# 🚀 Telegram Colored Inline Buttons with Premium Emoji

> Professional guide to Telegram Bot API 7.0+: Implement colored inline buttons with animated premium custom emoji icons using the modern MessageEntity system.

[![Telegram Bot API](https://img.shields.io/badge/Telegram%20API-v7.0+-26A5E4?logo=telegram)](https://core.telegram.org/bots/api)
[![Documentation](https://img.shields.io/badge/Docs-Official-blue)](https://core.telegram.org/bots/api#inlinekeyboardbutton)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success)]()

---

## 📑 Table of Contents

- [Overview](#overview)
- [What's New in Telegram Bot API 7.0](#whats-new-in-telegram-bot-api-70)
- [Colored Button Styles](#colored-button-styles)
- [Premium Emoji Icons](#premium-emoji-icons)
- [Entity System Deep Dive](#entity-system-deep-dive)
- [Button Placement Logic](#button-placement-logic)
- [Implementation Checklist](#implementation-checklist)
- [Best Practices](#best-practices)
- [Contact](#contact)

---

## 📖 Overview

Telegram Bot API 7.0 introduced a revolutionary update for bot developers: **Colored Inline Buttons** with **Premium Custom Emoji Icons**. This guide provides a comprehensive walkthrough of these new features using the modern **MessageEntity** system.

### Key Features Covered
- 🎨 **Glass Button Styles**: `primary` (blue), `success` (green), `danger` (red)
- ✨ **Premium Emoji Icons**: Animated custom emojis as button icons
- 🔬 **Entity-Based Processing**: Perfect formatting preservation without `parse_mode`
- 📐 **Dynamic Keyboard Layout**: Add buttons below or beside existing ones

---

## 🆕 What's New in Telegram Bot API 7.0

### The InlineKeyboardButton Evolution

Telegram has finally added native support for colored buttons and premium emoji icons in the Bot API. This eliminates the need for hacky workarounds and provides a clean, official way to create visually stunning bot interfaces.

### New Parameters

| Parameter | Type | Description | Example Value |
|:---|:---|:---|:---|
| `style` | String | Defines the button's visual style | `"primary"` |
| `icon_custom_emoji_id` | String | Premium animated emoji identifier | `"5392092877456258019"` |

### JSON Structure Example

```json
{
    "inline_keyboard": [
        [
            {
                "text": "Subscribe Now",
                "url": "https://t.me/DoxingAnalysis",
                "style": "primary",
                "icon_custom_emoji_id": "5392092877456258019"
            }
        ]
    ]
}
```

### API Request Example

```http
POST https://api.telegram.org/bot<TOKEN>/sendMessage
Content-Type: application/json

{
    "chat_id": 2115205796,
    "text": "Check out our premium channel!",
    "reply_markup": {
        "inline_keyboard": [
            [
                {
                    "text": "Join Channel",
                    "url": "https://t.me/DoxingAnalysis",
                    "style": "primary",
                    "icon_custom_emoji_id": "5392092877456258019"
                },
                {
                    "text": "Contact Admin",
                    "url": "https://t.me/WhiteDestrier",
                    "style": "success"
                }
            ],
            [
                {
                    "text": "Cancel",
                    "callback_data": "cancel",
                    "style": "danger"
                }
            ]
        ]
    }
}
```

---

## 🎨 Colored Button Styles

### Style Reference Guide

| Style | Hex Color | Use Case | Visual Effect |
|:---|:---|:---|:---|
| `primary` | `#2B6EE5` | Main CTAs, Navigation, Subscriptions | Solid blue background |
| `success` | `#0FA958` | Confirmations, Approvals, Positive actions | Solid green background |
| `danger` | `#E53935` | Deletions, Cancellations, Destructive actions | Solid red background |
| *(omitted)* | Transparent | Secondary actions, Neutral options | Subtle hover effect |

### Style Usage Guidelines

**Primary (Blue)**
- Use for the most important action on screen
- Maximum one primary button per message
- Examples: "Subscribe", "Get Started", "Learn More"

**Success (Green)**
- Use for positive confirmations
- Examples: "Confirm", "Approve", "Accept", "Yes"

**Danger (Red)**
- Use for destructive or irreversible actions
- Examples: "Delete", "Cancel", "Reject", "No"

**Default (No Style)**
- Use for secondary or neutral options
- Examples: "Back", "Settings", "Help"

### Visual Comparison

```
┌────────────────────────────────────────────┐
│  [ Subscribe ]  ← Primary (Blue)           │
├────────────────────────────────────────────┤
│  [ Confirm ]    ← Success (Green)          │
├────────────────────────────────────────────┤
│  [ Delete ]     ← Danger (Red)             │
├────────────────────────────────────────────┤
│  [ Cancel ]     ← Default (Transparent)    │
└────────────────────────────────────────────┘
```

---

## ✨ Premium Emoji Icons

### What Are Premium Custom Emojis?

Telegram Premium users can access exclusive animated emoji packs created by professional artists. These emojis are high-quality, smooth animations that can significantly enhance user engagement when used as button icons.

### Key Characteristics
- **Animated**: Smooth, eye-catching animations
- **Exclusive**: Only available to Telegram Premium subscribers
- **Unique IDs**: Each emoji has a unique string identifier
- **Icon-Only**: Can only be used as icons, not within text

### How to Obtain `custom_emoji_id`

#### Method 1: Extract from User Message (Dynamic)

When a user sends a premium emoji, Telegram includes it in the `entities` array of the message object.

**Incoming Webhook Payload:**
```json
{
    "update_id": 123456789,
    "message": {
        "message_id": 101,
        "from": {
            "id": 2115205796,
            "is_bot": false,
            "first_name": "WhiteDestrier"
        },
        "chat": {
            "id": 2115205796,
            "type": "private"
        },
        "date": 1704326400,
        "text": "⭐",
        "entities": [
            {
                "offset": 0,
                "length": 1,
                "type": "custom_emoji",
                "custom_emoji_id": "5392092877456258019"
            }
        ]
    }
}
```

**Extraction Logic (Pseudocode):**
```
function extractCustomEmojiId(message):
    if message.entities exists:
        for each entity in message.entities:
            if entity.type == "custom_emoji":
                return entity.custom_emoji_id
    
    if message.caption_entities exists:
        for each entity in message.caption_entities:
            if entity.type == "custom_emoji":
                return entity.custom_emoji_id
    
    return null
```

#### Method 2: Static Storage (Predefined Library)

Create a local JSON file containing your frequently used premium emoji IDs.

**emoji_library.json:**
```json
{
    "star_animated": "5392092877456258019",
    "heart_premium": "5312456789123456789",
    "fire_icon": "5234567890123456789",
    "crown_royal": "5456789123456789123",
    "diamond_blue": "5567891234567891234",
    "rocket_launch": "5678912345678912345",
    "check_green": "5789123456789123456",
    "cross_red": "5891234567891234567"
}
```

**Usage:**
```
emojiLibrary = loadJSON("emoji_library.json")
button.icon_custom_emoji_id = emojiLibrary.star_animated
```

### ⚠️ Critical Usage Rules

| ✅ Correct Usage | ❌ Incorrect Usage |
|:---|:---|
| Use `icon_custom_emoji_id` as button property | Insert emoji ID in `text` field |
| Keep button `text` clean and readable | Use HTML/Markdown entities for emoji in text |
| Extract ID from `entities` array | Hardcode unknown/unverified IDs |
| Pass ID as String type | Pass ID as Integer/Number type |

**Correct Implementation:**
```json
{
    "text": "Subscribe",
    "url": "https://t.me/channel",
    "icon_custom_emoji_id": "5392092877456258019"
}
```

**Incorrect Implementation:**
```json
{
    "text": "⭐ Subscribe",
    "url": "https://t.me/channel"
}
```

---

## 🔬 Entity System Deep Dive

### Why Entities > Parse Mode?

The traditional `parse_mode` approach has significant limitations that the Entity system elegantly solves.

| Legacy Approach | Modern Entity Approach |
|:---|:---|
| `parse_mode: "MarkdownV2"` | `entities: [...]` |
| Breaks with nested formatting | Preserves all formatting perfectly |
| Loses offset/length data | Maintains exact positioning |
| Cannot extract custom emoji IDs | Direct access to `custom_emoji_id` |
| Limited to one parse mode per message | Mixed formatting in single message |
| Fragile with special characters | Robust character handling |

### Entity Types Reference

| Entity Type | Description | Example |
|:---|:---|:---|
| `bold` | Bold text | `**text**` |
| `italic` | Italic text | `*text*` |
| `underline` | Underlined text | `__text__` |
| `strikethrough` | Strikethrough text | `~text~` |
| `spoiler` | Spoiler text | `\|\|text\|\|` |
| `blockquote` | Block quotation | `> text` |
| `code` | Inline code | `` `code` `` |
| `pre` | Code block | ` ```code``` ` |
| `text_link` | Hyperlink | `[text](url)` |
| `custom_emoji` | Premium emoji | Animated icon |

### Entity Extraction Logic

**Step-by-Step Algorithm:**

```
1. Receive Message object from Telegram webhook
2. Determine message type (text or media with caption)
3. Access appropriate entities array:
   - For text messages: message.entities
   - For media captions: message.caption_entities
4. Initialize empty array for valid entities
5. For each entity in entities array:
   - Check if entity.type is supported
   - If yes, add to valid entities array
   - If entity.type == "custom_emoji", store ID separately
6. Return processed entities and custom emoji IDs
```

### Complete Message Processing Example

**Input Message:**
```
Hello **World**! Check this ⭐ out!
```

**Raw API Response:**
```json
{
    "text": "Hello World! Check this ⭐ out!",
    "entities": [
        {
            "offset": 6,
            "length": 5,
            "type": "bold"
        },
        {
            "offset": 24,
            "length": 1,
            "type": "custom_emoji",
            "custom_emoji_id": "5392092877456258019"
        }
    ]
}
```

**Processed Output:**
- Text: `"Hello World! Check this ⭐ out!"`
- Bold range: Characters 6-10 ("World")
- Custom Emoji ID: `"5392092877456258019"` at position 24

---

## 🧩 Button Placement Logic

### Two-Dimensional Array Structure

The `inline_keyboard` is an **Array of Rows** where each row is an **Array of Buttons**.

**TypeScript Definition:**
```typescript
type InlineKeyboardButton = {
    text: string;
    url?: string;
    callback_data?: string;
    style?: "primary" | "success" | "danger";
    icon_custom_emoji_id?: string;
};

type InlineKeyboardMarkup = {
    inline_keyboard: InlineKeyboardButton[][];
};
```

### Visual Structure Guide

| Data Structure | Visual Result in Telegram |
|:---|:---|
| `[[A, B]]` | `[ A ] [ B ]` |
| `[[A], [B]]` | `[ A ]`<br>`[ B ]` |
| `[[A, B], [C]]` | `[ A ] [ B ]`<br>`[ C ]` |
| `[[A], [B, C, D]]` | `[ A ]`<br>`[ B ] [ C ] [ D ]` |
| `[[A, B], [C, D]]` | `[ A ] [ B ]`<br>`[ C ] [ D ]` |

### Placement Algorithms

#### 1. Add Button Below (New Row)

Creates a new row at the bottom with the new button.

```javascript
function addButtonBelow(keyboard, newButton) {
    // Create new row with the button
    const newRow = [newButton];
    
    // Append to keyboard
    keyboard.push(newRow);
    
    return keyboard;
}

// Usage
let keyboard = [[button1, button2]];
keyboard = addButtonBelow(keyboard, button3);
// Result: [[button1, button2], [button3]]
```

#### 2. Add Button Beside (Same Row)

Adds the new button to the last existing row.

```javascript
function addButtonBeside(keyboard, newButton) {
    if (keyboard.length === 0) {
        // If keyboard is empty, create first row
        keyboard.push([newButton]);
    } else {
        // Add to last row
        const lastRowIndex = keyboard.length - 1;
        keyboard[lastRowIndex].push(newButton);
    }
    
    return keyboard;
}

// Usage
let keyboard = [[button1]];
keyboard = addButtonBeside(keyboard, button2);
// Result: [[button1, button2]]
```

#### 3. Delete Last Button

Removes the most recently added button.

```javascript
function deleteLastButton(keyboard) {
    if (keyboard.length === 0) return keyboard;
    
    const lastRowIndex = keyboard.length - 1;
    const lastRow = keyboard[lastRowIndex];
    
    // Remove last button from row
    lastRow.pop();
    
    // If row becomes empty, remove the entire row
    if (lastRow.length === 0) {
        keyboard.pop();
    }
    
    return keyboard;
}

// Usage
let keyboard = [[button1, button2], [button3]];
keyboard = deleteLastButton(keyboard);
// Result: [[button1, button2]]
```

### Complete Layout Management Example

```javascript
class KeyboardBuilder {
    constructor() {
        this.keyboard = [];
    }
    
    addBelow(button) {
        this.keyboard.push([button]);
        return this;
    }
    
    addBeside(button) {
        if (this.keyboard.length === 0) {
            this.keyboard.push([button]);
        } else {
            this.keyboard[this.keyboard.length - 1].push(button);
        }
        return this;
    }
    
    deleteLast() {
        if (this.keyboard.length === 0) return this;
        
        const lastRow = this.keyboard[this.keyboard.length - 1];
        lastRow.pop();
        
        if (lastRow.length === 0) {
            this.keyboard.pop();
        }
        return this;
    }
    
    build() {
        return { inline_keyboard: this.keyboard };
    }
}

// Usage
const builder = new KeyboardBuilder();
builder
    .addBelow({ text: "Button 1", url: "https://example.com", style: "primary" })
    .addBeside({ text: "Button 2", url: "https://example.com", style: "success" })
    .addBelow({ text: "Button 3", url: "https://example.com", style: "danger" });
    
const replyMarkup = builder.build();
```

---

## ✅ Implementation Checklist

Use this checklist to ensure your implementation follows Telegram Bot API 7.0+ best practices.

### API Compatibility
- [ ] Confirm bot is using Bot API 7.0 or higher
- [ ] Test webhook endpoint with latest API version
- [ ] Update any deprecated methods

### Button Configuration
- [ ] Use exact style strings: `"primary"`, `"success"`, `"danger"`
- [ ] Pass `icon_custom_emoji_id` as String type (not Integer)
- [ ] Validate URLs before adding to buttons
- [ ] Ensure button text is clean (no premium emoji IDs in text)

### Entity Processing
- [ ] Extract entities from `message.entities` for text messages
- [ ] Extract entities from `message.caption_entities` for media
- [ ] Filter for supported entity types only
- [ ] Preserve offset and length values exactly

### JSON Encoding
- [ ] Properly JSON encode `reply_markup` before sending
- [ ] Use `Content-Type: application/json` header
- [ ] Handle special characters in button text

### Security
- [ ] Validate all incoming webhook data
- [ ] Sanitize user input for button text
- [ ] Implement rate limiting for production
- [ ] Use HTTPS for webhook endpoint

### Error Handling
- [ ] Check API response for `ok: true`
- [ ] Log failed requests for debugging
- [ ] Implement retry logic for transient failures
- [ ] Provide user-friendly error messages

---

## 🏆 Best Practices

### Performance Optimization

**Caching Strategy**
```
1. Store frequently used custom emoji IDs locally
2. Cache button configurations for repeated use
3. Use JSON serialization for session storage
4. Implement TTL (Time To Live) for cached data
```

**Request Optimization**
```
1. Batch multiple button updates when possible
2. Use editMessageReplyMarkup instead of sending new messages
3. Minimize keyboard size (max 100 buttons total)
4. Keep button text concise (1-3 words optimal)
```

### UX Guidelines

**Button Color Psychology**

| Color | Emotion | Best For | Avoid For |
|:---|:---|:---|:---|
| Blue (Primary) | Trust, Action | Subscribe, Next, Continue | Cancel, Delete |
| Green (Success) | Positive, Safe | Confirm, Accept, Yes | Neutral actions |
| Red (Danger) | Warning, Stop | Delete, Cancel, Reject | Primary CTAs |
| Default | Neutral | Back, Settings, More Info | Important actions |

**Premium Emoji Usage**

| Emoji Type | Best For | Example |
|:---|:---|:---|
| Star | Featured content | "Premium Access" |
| Crown | VIP/Admin features | "Admin Panel" |
| Fire | Trending/Popular | "Hot Deals" |
| Rocket | Launch/Start | "Get Started" |
| Check | Confirmations | "Approve" |
| Cross | Cancellations | "Reject" |

**Layout Best Practices**
```
✅ DO:
- Maximum 2-3 buttons per row on mobile
- Use Primary for main action (first button)
- Group related actions in same row
- Use Danger for destructive actions only

❌ DON'T:
- Mix too many colors in one keyboard
- Create rows with 4+ buttons (too small on mobile)
- Use Primary for cancel/destructive actions
- Overuse premium emoji icons (1-2 per message max)
```

### Code Organization

**Recommended File Structure:**
```
project/
├── src/
│   ├── Bot/
│   │   ├── KeyboardBuilder.js
│   │   ├── EntityProcessor.js
│   │   └── MessageSender.js
│   ├── Storage/
│   │   ├── SessionManager.js
│   │   └── EmojiLibrary.json
│   └── Utils/
│       ├── Logger.js
│       └── Validator.js
├── config/
│   └── bot.config.json
├── webhook/
│   └── index.js
└── README.md
```

### Production Checklist

- [ ] SSL certificate configured for webhook
- [ ] Error monitoring service integrated
- [ ] Log rotation implemented
- [ ] Session data backed up regularly
- [ ] Rate limit handling in place
- [ ] Graceful degradation for API failures
- [ ] Health check endpoint available

---

## 👤 Contact

**Developer & Maintainer**

- **Telegram**: [@WhiteDestrier](https://t.me/WhiteDestrier)
- **Channel**: [@DoxingAnalysis](https://t.me/DoxingAnalysis)

**Resources**

- [Official Telegram Bot API Documentation](https://core.telegram.org/bots/api)
- [InlineKeyboardButton Reference](https://core.telegram.org/bots/api#inlinekeyboardbutton)
- [MessageEntity Reference](https://core.telegram.org/bots/api#messageentity)

---

```
