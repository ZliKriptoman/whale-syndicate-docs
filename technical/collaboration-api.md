# 🤝 Collaboration API

API for cross-promotion task integration with other Telegram mini app games.

---

## 📋 Overview

Whale Syndicate provides a public API that enables:

1. **Partner games** to verify if a user completed a task in Whale Syndicate
2. **Your game** to verify if a user completed a task in partner games

This enables seamless cross-promotion between Telegram mini apps.

---

## 🔗 Public API Endpoint

### Check Task Completion

**Endpoint:** `GET /check-task-completion`

Partner games call this endpoint to check if a user completed a task in Whale Syndicate.

**Base URL:**
```
https://whaleverify.zli-kriptoman.workers.dev
```

**Request:**
```http
GET /check-task-completion?userId=123456789&taskId=whale_syndicate_task
```

**Query Parameters:**
- `userId` (required) - Telegram user ID (numeric)
- `taskId` (required) - Task ID to check (e.g. `whale_syndicate_task`)

**Example:**
```bash
curl "https://whaleverify.zli-kriptoman.workers.dev/check-task-completion?userId=123456789&taskId=whale_syndicate_task"
```

**Success Response (Task Completed):**
```json
{
  "ok": true,
  "userId": 123456789,
  "taskId": "whale_syndicate_task",
  "completed": true,
  "completedAt": "2026-02-12T10:30:00.000Z"
}
```

**Success Response (Task Not Completed):**
```json
{
  "ok": true,
  "userId": 123456789,
  "taskId": "whale_syndicate_task",
  "completed": false,
  "completedAt": null
}
```

**Error Response:**
```json
{
  "ok": false,
  "error": "MISSING_PARAMS" | "USER_NOT_FOUND" | "SERVER_ERROR"
}
```

---

## 🔐 Security & Privacy

### Is it safe to be public?

**Yes.** This endpoint is designed to be public and safe:

✅ **Read-only operation** - Only checks task completion, doesn't modify data  
✅ **No sensitive data exposed** - Only returns completion status  
✅ **Rate limiting** - Protected against abuse  
✅ **No authentication required** - Simplifies partner integration  

**What information is exposed?**
- Only whether a specific user completed a specific task
- No personal data, scores, or other game information

**What can't be done?**
- Cannot modify user data
- Cannot claim rewards
- Cannot access other user information
- Cannot bypass game mechanics

---

## 💻 Integration Example

### JavaScript/Node.js

```javascript
/**
 * Check if user completed Whale Syndicate task
 * @param {number} userId - Telegram user ID
 * @param {string} taskId - Task ID (e.g. 'whale_syndicate_task')
 * @returns {Promise<boolean>} - True if task is completed
 */
async function checkWhaleSyndicateTask(userId, taskId = 'whale_syndicate_task') {
    try {
        const url = `https://whaleverify.zli-kriptoman.workers.dev/check-task-completion?userId=${userId}&taskId=${taskId}`;
        const response = await fetch(url);
        const data = await response.json();
        
        return data.ok && data.completed === true;
    } catch (error) {
        console.error('Error checking Whale Syndicate task:', error);
        return false;
    }
}

// Usage in partner's verify-task endpoint
app.post('/verify-task', async (req, res) => {
    const { userId, taskId } = req.body;
    
    // If task is "whale_syndicate_task", verify with Whale Syndicate
    if (taskId === 'whale_syndicate_task') {
        const completed = await checkWhaleSyndicateTask(userId);
        
        if (!completed) {
            return res.json({
                ok: false,
                completed: false,
                error: 'User has not completed Whale Syndicate task'
            });
        }
    }
    
    // Continue with verification and reward...
    res.json({ ok: true, completed: true });
});
```

### Python

```python
import requests

def check_whale_syndicate_task(user_id, task_id='whale_syndicate_task'):
    """
    Check if user completed Whale Syndicate task
    """
    url = f"https://whaleverify.zli-kriptoman.workers.dev/check-task-completion"
    params = {
        'userId': user_id,
        'taskId': task_id
    }
    
    try:
        response = requests.get(url, params=params)
        data = response.json()
        return data.get('ok') and data.get('completed') == True
    except Exception as e:
        print(f'Error checking Whale Syndicate task: {e}')
        return False
```

---

## 🎯 Use Cases

### Multiple Partners

**Yes, this API supports multiple partners!**

Each partner uses their own `taskId`:
- Partner A: `taskId=whale_syndicate_task_partner_a`
- Partner B: `taskId=whale_syndicate_task_partner_b`
- Partner C: `taskId=whale_syndicate_task_partner_c`

The API checks each task independently. You can have unlimited partners.

### Example: Multiple Partners

```javascript
// Partner A checks their task
const partnerACompleted = await checkWhaleSyndicateTask(userId, 'whale_syndicate_task_partner_a');

// Partner B checks their task
const partnerBCompleted = await checkWhaleSyndicateTask(userId, 'whale_syndicate_task_partner_b');

// Both are independent
```

---

## 📊 Rate Limits

- **No strict rate limits** for read-only operations
- **Reasonable usage expected** - Don't spam requests
- **Abuse protection** - Excessive requests may be rate-limited

---

## 🚀 Getting Started

### For Partners

1. **Agree on Task ID** - Decide on a unique task ID (e.g. `whale_syndicate_task_yourgame`)

2. **Add Task in Whale Syndicate** - Contact us to add your task to the game

3. **Integrate API** - Use the code examples above to check task completion

4. **Test** - Verify integration works before going live

### For Whale Syndicate

To add a new partner task:

1. Add task to frontend (`src/frontend/index.html`):
```javascript
const TASKS = [
    // ... existing tasks ...
    { 
        id: 'external_partner_name',
        icon: '🎮',
        title: 'Try Partner Game',
        reward: 150000,
        link: 'https://t.me/partner_bot/app'
    }
];
```

2. Deploy - Task will be available to users

---

## ❓ FAQ

### Can this API be used for multiple partners?

**Yes!** Each partner gets their own unique `taskId`. The API supports unlimited partners.

### Is it safe to be public?

**Yes!** The endpoint is read-only and only exposes task completion status. No sensitive data is exposed.

### Does this affect existing game functions?

**No!** This is a completely separate endpoint that doesn't interfere with:
- User authentication
- Game mechanics
- Score calculations
- Other API endpoints

### What happens if the API is down?

- Partner games should handle errors gracefully
- Users won't be able to verify tasks temporarily
- Existing game functions continue to work normally

### Can partners abuse this API?

- Only read operations are possible
- No data modification possible
- Rate limiting protects against abuse
- Only task completion status is exposed

---

## 📞 Support

For integration support or questions:
- Contact: [Telegram Support](https://t.me/WhaleSyndicate_CHAT)
- Technical Issues: Open an issue in our repository

---

## 📝 Changelog

### v1.0.0 (2026-02-12)
- Initial API release
- Task completion verification endpoint
- Support for multiple partners

---

**Ready to integrate?** Use the code examples above and start building! 🚀
