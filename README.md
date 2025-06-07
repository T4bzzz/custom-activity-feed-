# StreamElements Activity Feed

A beautiful, real-time activity feed for StreamElements that displays donations, subscriptions, follows, raids, and other stream events with a modern glassmorphism design.

## Features

### Real-Time Event Display
- **Donations/Tips**: Shows amount, user, and message
- **Subscriptions**: Displays tier and subscription messages
- **Follows**: Real-time follower notifications
- **Raids**: Shows raider count and username
- **Hosts**: Displays host information and viewer count
- **Cheers/Bits**: Shows bit amount and messages

### User Interface
- Modern glassmorphism design with gradient backgrounds
- Smooth animations and hover effects
- Responsive design for different screen sizes
- Dark theme with colorful accent gradients
- Real-time status indicators

### Functionality
- **Mark New Mode**: Flag new activities while away and jump to oldest new item
- **Sound Notifications**: Simple audio alert when new events arrive
- **Mute/Unmute**: Toggle notification sound on/off
- **Clear Feed**: Reset the activity feed
- **Auto-Reconnect**: Automatic reconnection with exponential backoff
- **Popout Window**: Clean version perfect for OBS browser sources

### OBS Integration
- Dedicated popout version for streaming software
- Clean interface without navigation elements
- Optimized for browser source capture
- Transparent background support

## Quick Setup

### Prerequisites
- Active StreamElements account
- Twitch channel
- Modern web browser

### Option 1: Use Hosted Version (Recommended)

1. **Visit the hosted application**
   ```
   https://t4bzzz.tech/loginactivityfeed
   ```

2. **Get your StreamElements JWT Token**
   - Go to your StreamElements Dashboard
   - Navigate to Settings
   - Find the "JWT TOKEN" section
   - Copy the token value

3. **Configure the application**
   - Enter your Twitch channel name
   - Paste your StreamElements JWT token
   - Click "Save Configuration"
   - Test the connection

4. **Launch the feed**
   - Click "Launch Activity Feed" for the full version
   - Click "Open Popout Version" for OBS browser source

### Option 2: Self-Hosted Installation

1. **Download the files**
   ```bash
   git clone https://github.com/yourusername/streamelements-activity-feed.git
   cd streamelements-activity-feed
   ```

2. **Host the files**
   - Upload to your web server, or
   - Use a local server like Live Server extension in VS Code, or
   - Use Python: `python -m http.server 8000`

3. **Follow steps 2-4 from Option 1** using your local URL

## File Structure

```
streamelements-activity-feed/
├── login activity feed.html    # Setup and configuration page
├── activity feed.html          # Main activity feed interface
├── activity-feed-popout.html   # Popout version for OBS
├── dein-logo.png              # Logo image (replace with your own)
└── README.md                  # This file
```

## Configuration

### StreamElements Setup

1. **JWT Token**: 
   - Required for authentication with StreamElements API
   - Found in StreamElements Dashboard > Settings
   - Starts with "eyJ" and is a long encoded string

2. **Channel Name**:
   - Your Twitch username (lowercase)
   - Must match exactly with your StreamElements connected channel

### Application Settings

Settings are automatically saved in browser cookies:

- **streamElementsToken**: Your JWT authentication token
- **twitchChannel**: Your Twitch channel name  
- **soundMuted**: Sound notification preference

### Browser Compatibility

- **Chrome/Chromium**: Full support
- **Firefox**: Full support
- **Safari**: Full support
- **Edge**: Full support

## Usage Guide

### Main Interface (`activity feed.html`)

**Controls:**
- **Mark New**: Start/stop flagging new activities
- **Sound**: Toggle simple notification sound for new events
- **Clear Feed**: Remove all current activities
- **Back**: Return to setup page
- **Popout**: Open OBS-friendly version

**Features:**
- Displays up to 50 recent activities
- Auto-scrolling activity feed
- Real-time connection status
- Last update timestamp

### Popout Version (`activity-feed-popout.html`)

**Optimized for OBS:**
- Clean interface without navigation
- Automatic setup validation
- Same real-time functionality
- Minimal visual elements
- Perfect for browser source capture

### OBS Browser Source Setup

**Using Hosted Version:**
1. Add new Browser Source in OBS
2. Set URL to: `https://t4bzzz.tech/activity-feed-popout.html`
3. Set Width: 500px, Height: 800px
4. Enable "Shutdown source when not visible" for performance
5. Enable "Refresh browser when scene becomes active"

**Using Self-Hosted Version:**
1. Add new Browser Source in OBS
2. Set URL to: `http://your-server/activity-feed-popout.html`
3. Follow steps 3-5 from above

## API Integration

### StreamElements WebSocket

The application connects to StreamElements real-time API:

```javascript
// Connection endpoint
wss://realtime.streamelements.com/socket.io/?EIO=3&transport=websocket

// Authentication message
420["authenticate",{"method":"jwt","token":"YOUR_JWT_TOKEN"}]

// Channel join message  
421["join-channel",{"channel":"YOUR_CHANNEL"}]
```

### Event Types Supported

- `tip` / `donation`: Donation events
- `subscriber` / `resub`: Subscription events
- `follow`: New follower events
- `cheer` / `bits`: Bit/cheer events
- `raid`: Raid events
- `host`: Host events

## Customization

### Styling

The application uses CSS custom properties for easy theming:

```css
:root {
    --primary-gradient: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
    --accent-gradient: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
    --glass-bg: rgba(255, 255, 255, 0.05);
    /* ... more variables */
}
```

### Logo Replacement

Replace `dein-logo.png` with your own logo:
- Recommended size: 80px height
- Supports PNG with transparency
- Automatically applies drop shadow effect

### Sound Notification

The application plays a simple notification sound when new events arrive. You can modify the sound properties in the `playNotificationSound()` function:

```javascript
// Simple tone generation with different frequencies per event type
const frequencies = {
    donation: 800,    // Higher pitch for donations
    subscription: 600, // Medium pitch for subscriptions  
    follow: 400,      // Lower pitch for follows
    cheer: 700,       // High pitch for cheers
    raid: 500,        // Medium-low pitch for raids
    host: 450         // Low-medium pitch for hosts
};
```

## Troubleshooting

### Connection Issues

**"Setup Required" Error:**
- Verify JWT token is correct and not expired
- Check channel name matches StreamElements configuration
- Clear browser cookies and reconfigure

**"Connection Lost" Status:**
- Check internet connection
- Verify StreamElements service status
- Wait for automatic reconnection (up to 15 attempts)

**No Events Appearing:**
- Ensure StreamElements is properly connected to your Twitch
- Test with StreamElements Activity Feed to verify events are working
- Check browser console for error messages

### Browser Issues

**Notification Sound Not Playing:**
- Enable audio autoplay in browser settings
- Interact with page before expecting audio (click somewhere first)
- Check if notification sound is muted in application
- Verify browser audio permissions

**Cookies Not Saving:**
- Ensure cookies are enabled in browser
- Check if using HTTPS (required for some browsers)
- Clear browser cache and retry setup

### OBS Integration Issues

**Browser Source Not Loading:**
- Verify URL is accessible from OBS machine
- Check firewall settings
- Use local IP address instead of localhost if needed

**Performance Issues:**
- Enable "Shutdown source when not visible" in OBS
- Reduce browser source resolution if needed
- Close unnecessary browser tabs

## Development

### Local Development

**Note**: For most users, the hosted version at `https://t4bzzz.tech/loginactivityfeed` is recommended.

1. **Clone repository**
   ```bash
   git clone <repository-url>
   cd streamelements-activity-feed
   ```

2. **Start local server**
   ```bash
   # Python 3
   python -m http.server 8000
   
   # Node.js
   npx http-server
   
   # PHP
   php -S localhost:8000
   ```

3. **Access application**
   ```
   http://localhost:8000/login%20activity%20feed.html
   ```

### Code Structure

**HTML Files:**
- Self-contained with embedded CSS and JavaScript
- No external dependencies except Font Awesome CDN
- Responsive design with mobile support

**Key JavaScript Functions:**
- `connectToStreamElements()`: WebSocket connection management
- `handleStreamElementsEvent()`: Event processing and display
- `addActivity()`: Activity item creation and management
- `playNotificationSound()`: Audio notification system

## Security Notes

- JWT tokens are stored locally in browser cookies only
- No data is sent to external servers except StreamElements
- Tokens are never logged or transmitted to third parties
- Use HTTPS in production for secure cookie storage

## Performance

- **Memory Usage**: Automatically limits to 50 activities maximum
- **Reconnection**: Exponential backoff prevents server flooding
- **Animations**: Hardware-accelerated CSS transforms
- **Network**: Minimal WebSocket traffic with heartbeat

## Browser Storage

The application uses browser cookies for configuration:

- **Duration**: 30 days expiration
- **Scope**: Path-specific, secure flags when using HTTPS
- **Privacy**: No tracking or analytics data stored

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly with StreamElements
5. Submit a pull request

## Support

- **GitHub Issues**: Report bugs and feature requests
- **Discord**: Join the community server
- **Twitch**: Follow @t4bzzz_ for updates

## Credits

**Created by:** @t4bzzz_

**Technologies Used:**
- HTML5 / CSS3 / JavaScript
- StreamElements WebSocket API  
- Font Awesome Icons
- CSS Grid and Flexbox
- Web Audio API

## License

This project is open source. See LICENSE file for details.

---

**Note:** This application requires a valid StreamElements account and JWT token. StreamElements terms of service apply to API usage.
