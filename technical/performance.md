---
description: The technical wizardry behind Whale Syndicate - Fast, secure, and scalable
---

# ⚡ Performance & Technology

## The Stack

Whale Syndicate runs on a modern, serverless architecture designed for speed, security, and scale.

**Translation:** It's fast, it doesn't break, and it handles thousands of concurrent tappers without breaking a sweat.

***

## 🏗️ Architecture Overview

### The Three-Tier System

**Frontend** → **Backend** → **Database**
```
┌─────────────────────────────────────────────┐
│          Cloudflare Pages (Frontend)        │
│   • React/HTML/JS                          │
│   • Sub-100ms load times globally         │
│   • CDN-distributed                        │
└─────────────────────────────────────────────┘
                    ↓ HTTPS
┌─────────────────────────────────────────────┐
│       Cloudflare Workers (Backend)          │
│   • Edge computing (low latency)           │
│   • Server-side validation                 │
│   • Payment processing                     │
│   • Real-time leaderboards                 │
└─────────────────────────────────────────────┘
                    ↓ PostgreSQL
┌─────────────────────────────────────────────┐
│          Supabase (Database)                │
│   • User profiles & progress               │
│   • Transaction history                    │
│   • Leaderboard data                       │
│   • Real-time subscriptions                │
└─────────────────────────────────────────────┘
```

{% hint style="success" %}
**Why This Stack?**

- **Fast:** Edge computing = <50ms response times
- **Scalable:** Handles 10K+ concurrent users easily
- **Reliable:** 99.99% uptime (Cloudflare SLA)
- **Secure:** Server-side validation prevents cheating
{% endhint %}

***

## 🚀 Performance Metrics

### Load Times

**Initial Page Load:**
- Cold start: **<500ms**
- Cached: **<100ms**

**API Requests:**
- Average: **<50ms**
- 95th percentile: **<200ms**

**Leaderboard Updates:**
- Refresh: **<100ms**
- Real-time: **<5s latency**

### Throughput

**Concurrent Users:**
- Current peak: **~5,000 simultaneous**
- Tested capacity: **50,000+**

**Requests Per Day:**
- Average: **~1,000,000 requests**
- Peak days: **5,000,000+ requests**

**Happy Fifth Events:**
- 04:00 UTC spike: **~3,000 concurrent tappers**
- 16:00 UTC spike: **~4,000 concurrent tappers**

{% hint style="info" %}
**Translation:**

The game handles thousands of players tapping simultaneously without lag.

Even during Happy Fifth chaos, you won't notice slowdowns.
{% endhint %}

***

## 🔒 Security Architecture

### Authentication

**Telegram OAuth 2.0:**
- No passwords (Telegram handles auth)
- Secure token-based sessions
- Auto-expiry (24-hour tokens)

**No Personal Data Stored:**
- Only Telegram ID + username
- No email, phone, or personal info
- GDPR compliant

### Transaction Security

**TON Payments:**
- On-chain verification
- Smart contract validation
- Double-spend prevention

**Telegram Stars:**
- Telegram native payment API
- Instant verification
- Fraud detection

### Anti-Cheat Measures

**Server-Side Validation:**
```javascript
// ALL game logic runs on server
// Client can't manipulate values

validateTap(userId, timestamp) {
  // Check rate limits
  if (exceedsRateLimit(userId)) return false;
  
  // Verify energy available
  if (energy < 1) return false;
  
  // Validate tap power
  if (tapValue !== expectedValue) return false;
  
  return true;
}
```

**Rate Limiting:**
- Max 20 actions per minute
- Sliding window algorithm
- Automatic cooldown for violations

**Pattern Detection:**
- Unusual tap speeds flagged
- Suspicious patterns investigated
- Auto-bans for confirmed cheaters

{% hint style="warning" %}
**For Cheaters:**

We see everything server-side. Auto-clickers, bots, and exploits are detected automatically.

**First offense:** Warning
**Second offense:** 24-hour ban
**Third offense:** Permanent ban

Don't bother trying. It's not worth it.
{% endhint %}

***

## 💰 Payment Processing

### TON Integration

**TonConnect SDK:**
- Direct wallet connection
- No custodial risk
- ~5-second confirmations

**Supported Wallets:**
- Tonkeeper
- MyTonWallet
- Tonhub
- OpenMask

**Transaction Flow:**
```
1. User clicks "Buy with TON"
2. TonConnect popup (wallet selection)
3. User approves in wallet app
4. Transaction broadcast to TON blockchain
5. Backend monitors blockchain
6. Confirmation (typically 5-10 seconds)
7. Item credited to account
```

### Telegram Stars

**Native Integration:**
- Telegram's in-app payment system
- Instant confirmation
- No blockchain needed

**Dynamic Pricing:**
```javascript
// Pricing updates every 60 seconds
const tonPrice = await fetchCoinGeckoPrice("the-open-network");
const starsRate = tonPrice * 100; // 1 TON ≈ 100 Stars

// Example: Leverage 1 = 5 TON
const leverageStarsPrice = 5 * starsRate;
```

**Why Dynamic?**
- TON/Stars rate fluctuates
- Fair pricing regardless of volatility
- Always equivalent value

***

## 📊 Data Management

### Database Schema

**Core Tables:**

**users**
```sql
id: bigint (Telegram ID)
username: text
level: integer (0-100)
balance: bigint
energy: integer
created_at: timestamp
```

**transactions**
```sql
id: uuid
user_id: bigint
type: text ('tap', 'upgrade', 'purchase')
amount: bigint
timestamp: timestamp
```

**leaderboard**
```sql
user_id: bigint
score: bigint
rank: integer
updated_at: timestamp
```

### Caching Strategy

**KV Cache (60-second TTL):**
- User balance
- Leaderboard positions
- Active multipliers

**Why Cache?**
- Reduces database load
- Faster response times
- Better user experience

**Cache Invalidation:**
- On user action (tap, purchase, etc.)
- Automatic after 60 seconds
- Manual purge for critical updates

***

## 🌐 Global Distribution

### CDN Coverage

**Cloudflare Network:**
- 300+ data centers worldwide
- Automatic routing to nearest edge
- Sub-50ms latency globally

**Regional Performance:**

| Region | Avg Latency |
|--------|-------------|
| Europe | 20-40ms |
| North America | 30-50ms |
| Asia | 40-70ms |
| South America | 60-90ms |
| Africa | 80-120ms |

{% hint style="success" %}
**No matter where you are, the game is fast.**

Whether you're in London or Lagos, Tokyo or Toronto — same speed.
{% endhint %}

***

## 📱 Device Compatibility

### Supported Platforms

**Mobile:**
- ✅ iOS 13+ (iPhone, iPad)
- ✅ Android 8.0+ (all devices)
- ✅ Telegram Desktop (Windows, macOS, Linux)

**Browsers:**
- ✅ Chrome/Edge (Chromium)
- ✅ Safari (iOS/macOS)
- ✅ Firefox
- ✅ Telegram in-app browser

**Screen Sizes:**
- ✅ Mobile (320px - 480px)
- ✅ Tablet (481px - 1024px)
- ✅ Desktop (1025px+)

### Progressive Web App (PWA)

**Features:**
- ✅ Offline mode (cached assets)
- ✅ Add to home screen
- ✅ Push notifications (via Telegram)
- ✅ Background sync (passive income)

***

## ⚙️ Game State Sync

### Real-Time Updates

**WebSocket Connections:**
- Persistent connection for active players
- Instant balance updates
- Live leaderboard changes

**Polling Fallback:**
- Every 5 seconds (if WebSocket fails)
- Ensures state consistency
- Automatic reconnection

### Multi-Device Support

**Seamless Sync:**
```
Phone → Tap 1,000 times → Close app
Tablet → Open app → Balance updated ✅
```

**How it works:**
- All state stored server-side
- Telegram ID = unique identifier
- Device-agnostic (works everywhere)

{% hint style="info" %}
**Play on phone, continue on tablet, check on desktop.**

Your progress follows you everywhere.
{% endhint %}

***

## 🛡️ Reliability & Uptime

### Service Level Agreement (SLA)

**Cloudflare Workers:**
- 99.99% uptime guarantee
- <1 hour downtime per year

**Supabase:**
- 99.9% uptime guarantee
- Automatic backups (hourly)

**Actual Performance (Last 90 Days):**
- Uptime: **99.97%**
- Incidents: **0 major outages**
- Average response time: **<50ms**

### Monitoring & Alerts

**Real-Time Monitoring:**
- Error rate tracking
- Response time metrics
- User activity patterns

**Automatic Alerts:**
- SMS/Email on critical errors
- Slack notifications for anomalies
- Auto-scaling triggers

***

## 🔧 Backend Infrastructure

### Cloudflare Workers

**Why Workers?**
- Edge computing (deployed globally)
- Zero cold starts (unlike Lambda)
- 10ms CPU time limit (forces efficiency)

**Worker Script Stats:**
- Lines of code: **~2,000**
- Execution time: **<10ms average**
- Memory usage: **<10MB**

### Environment Variables

**Secure Secrets Management:**
```javascript
// Never hardcoded in code
const SUPABASE_URL = env.SUPABASE_URL;
const SUPABASE_KEY = env.SUPABASE_ANON_KEY;
const TON_RECEIVER = env.TON_RECEIVER_ADDRESS;
```

**Why Important?**
- API keys never exposed
- Can rotate without code changes
- Environment-specific configs (dev/prod)

***

## 🎮 Game Logic Optimization

### Tap Validation

**Server-Side Processing:**
```javascript
// Tap event received
POST /api/tap
{
  userId: 123456789,
  timestamp: 1703001234,
  signature: "abc123..."
}

// Server validates:
1. User exists ✅
2. Energy available ✅
3. Tap within rate limit ✅
4. Signature valid ✅
5. Timestamp recent (<5s) ✅

// Then processes:
- Deduct 1 energy
- Add coins (level × leverage × multiplier)
- Update database
- Return new state
```

**Why Server-Side?**
- Client can't fake balance
- Prevents cheating
- Consistent state globally

### Happy Fifth Multiplier

**Real-Time Activation:**
```javascript
// 04:00 UTC hits
setInterval(() => {
  const now = new Date();
  const hour = now.getUTCHours();
  const minute = now.getUTCMinutes();
  
  if ((hour === 4 || hour === 16) && minute < 12) {
    activateMultiplier();
  } else {
    deactivateMultiplier();
  }
}, 10000); // Check every 10 seconds
```

**Global Synchronization:**
- All servers check UTC time
- Multiplier active for ALL users simultaneously
- No time zone advantage

***

## 📈 Scalability

### Current Capacity

**Daily Active Users:**
- Current: **~10,000 DAU**
- Tested: **100,000 DAU**
- Maximum: **1,000,000+ DAU** (with auto-scaling)

**Database Capacity:**
- Current rows: **~500,000**
- Maximum: **100,000,000+** (Supabase scale)

### Auto-Scaling

**Cloudflare Workers:**
- Automatic scaling (no config needed)
- Pay-per-request model
- No "server full" errors

**Database:**
- Connection pooling
- Read replicas (for leaderboards)
- Automatic failover

{% hint style="success" %}
**Translation:**

Even if the game goes viral and 100,000 people start playing tomorrow, it won't break.

The architecture scales automatically.
{% endhint %}

***

## 🐛 Error Handling

### Graceful Degradation

**If Server Unavailable:**
- Show cached balance (last known state)
- Queue actions locally
- Sync when connection restored

**If Database Slow:**
- Return cached data
- Background sync continues
- User experience unaffected

**If Payment Fails:**
- Retry automatically (3 attempts)
- Refund if charged but not delivered
- Manual support if still fails

### Logging & Debugging

**Error Tracking:**
- All errors logged to Sentry
- User context included
- Stack traces preserved

**Debug Mode:**
- Available for admins
- Real-time request inspection
- Performance profiling

***

## 🔬 Testing & Quality Assurance

### Automated Testing

**Unit Tests:**
- Core game logic
- Payment processing
- Anti-cheat systems

**Integration Tests:**
- API endpoints
- Database operations
- Webhook handling

**Load Tests:**
- Simulated 10K concurrent users
- Happy Fifth stress tests
- Payment system capacity

### Manual QA

**Pre-Release Checklist:**
- ✅ All features tested
- ✅ Payment flows verified
- ✅ Mobile responsiveness checked
- ✅ Cross-browser compatibility
- ✅ Security audit passed

***

## 📞 Technical Support

### For Developers

**API Documentation:**
- Coming soon (maybe)

**Open Source:**
- Not yet (but considering)

**Bug Reports:**
- [@WSPORTAL](https://t.me/WSPORTAL)

### For Players

**Common Issues:**

<details>
<summary><strong>Game not loading?</strong></summary>

**Try:**
1. Clear browser cache
2. Check internet connection
3. Try different browser
4. Restart Telegram app

Still broken? Report in chat.

</details>

<details>
<summary><strong>Balance not updating?</strong></summary>

**Causes:**
- Slow connection (wait 5-10s)
- Server cache (wait 60s)
- Actual bug (report it)

**Fix:** Refresh page. If still broken, contact support.

</details>

<details>
<summary><strong>Payment not processed?</strong></summary>

**Wait 5 minutes** (blockchain confirmation)

If still not credited:
1. Screenshot transaction ID
2. Report in [@WSPORTAL](https://t.me/WSPORTAL)
3. We'll investigate manually

</details>

***

## 🎯 Future Improvements

### Roadmap (Maybe)

**Planned Enhancements:**
- ⏳ GraphQL API (faster queries)
- ⏳ WebSocket improvements (lower latency)
- ⏳ AI anti-cheat (ML-based detection)
- ⏳ Voice commands (jk, probably not)

**No Timeline:**
- We ship when ready
- Quality > speed
- No promises

{% hint style="info" %}
**We're constantly improving.**

But we don't announce features until they're ready.

**Surprise > hype**
{% endhint %}

***

## 🎩 Final Notes

### Why This Matters

Most tap-to-earn games are built on shaky foundations:
- Slow servers
- Frequent downtime
- Exploitable code
- Poor security

**We're different:**
- Enterprise-grade infrastructure
- Security-first design
- Scalable architecture
- Fast everywhere

**Translation:**

This game is built to last. Not a quick cash grab.

We're in it for the long haul.

***

**Next:** [FAQ](../support/faq.md) → [Contact & Links](../support/contact.md)

***

_"Built with precision. Powered by cloud. Secured by paranoia." ⚡🎩_