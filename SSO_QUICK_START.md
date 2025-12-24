# SSO Quick Start Guide

## What Has Been Implemented

Your SystemtwoIT system now supports Single Sign-On (SSO) authentication with your external system running at http://127.0.0.1:8000.

## Files Created/Modified

### New Files Created:
1. `app/Http/Controllers/SSOController.php` - Handles SSO authentication
2. `app/Http/Middleware/SSOAuthenticate.php` - Auto-login middleware
3. `config/sso.php` - SSO configuration
4. `README_SSO_INTEGRATION.md` - Complete documentation
5. `EXTERNAL_SYSTEM_SSO_SETUP.md` - External system implementation guide

### Modified Files:
1. `routes/web.php` - Added SSO routes
2. `.env` - Added SSO configuration
3. `resources/views/auth/login.blade.php` - Added SSO login button and auto-check

## Quick Setup (5 Steps)

### Step 1: Update Shared Secret
Edit `.env` and change the shared secret to something secure:
```env
SSO_SHARED_SECRET=your-super-secret-key-123456
```

### Step 2: Clear Cache
```bash
php artisan config:clear
php artisan cache:clear
```

### Step 3: Setup External System
Follow the complete guide in `EXTERNAL_SYSTEM_SSO_SETUP.md` to implement the required API endpoints in your external system (http://127.0.0.1:8000).

### Step 4: Test the Integration
1. Start external system: `php artisan serve` (port 8000)
2. Start SystemtwoIT: `php artisan serve --port=8001`
3. Login to external system
4. Visit SystemtwoIT - you should auto-login!

### Step 5: Production Configuration (When Ready)
Update `.env` for production:
```env
SSO_EXTERNAL_SYSTEM_URL=https://your-production-url.com
SSO_SHARED_SECRET=your-strong-production-secret
SSO_AUTO_LOGIN_ENABLED=true
```

## How It Works

### Auto-Login Flow:
1. User logs into external system (http://127.0.0.1:8000)
2. User visits SystemtwoIT system
3. JavaScript checks if user is authenticated in external system
4. If yes, automatically logs user into SystemtwoIT
5. User is redirected to dashboard

### Manual SSO Button:
- Login page now has "Login with SSO" button
- Redirects to external system if not authenticated there
- Then returns to SystemtwoIT with authentication

## Configuration Options

All in `.env`:

```env
# SSO System URL
SSO_EXTERNAL_SYSTEM_URL=http://127.0.0.1:8000

# Shared secret (must match external system)
SSO_SHARED_SECRET=your-shared-secret-key-here

# Auto-create users if they don't exist (true/false)
SSO_AUTO_PROVISION_USERS=false

# Enable automatic login (true/false)
SSO_AUTO_LOGIN_ENABLED=true

# Token validity in seconds
SSO_TOKEN_EXPIRATION=60

# Debug logging (true/false)
SSO_DEBUG_MODE=false
```

## Security Notes

⚠️ **IMPORTANT**:
- The `SSO_SHARED_SECRET` must be identical in both systems
- Keep the shared secret secure - never commit it to version control
- Use HTTPS in production
- Token expiration is set to 60 seconds by default for security

## Testing

### Test Auto-Login:
1. Make sure both systems are running
2. Login to http://127.0.0.1:8000
3. Open new tab to http://127.0.0.1:8001
4. Should auto-login!

### Test Manual SSO Button:
1. Logout from SystemtwoIT
2. Click "Login with SSO" button on login page
3. Should redirect to external system
4. After logging in there, return to SystemtwoIT

### Test API Endpoints:
```bash
# Check session (from external system)
curl http://127.0.0.1:8000/api/sso/check-session

# Verify token
curl -X POST http://127.0.0.1:8000/api/sso/verify \
  -H "Content-Type: application/json" \
  -d '{"token":"test","secret":"your-shared-secret-key-here"}'
```

## Troubleshooting

### "Invalid SSO token"
- Check `SSO_SHARED_SECRET` matches in both systems
- Run `php artisan config:clear` on both systems

### Auto-login not working
- Verify `SSO_AUTO_LOGIN_ENABLED=true`
- Check if user exists in SystemtwoIT system
- Check browser console for JavaScript errors

### External system not responding
- Ensure external system is running
- Verify `SSO_EXTERNAL_SYSTEM_URL` is correct
- Check network connectivity

## Next Steps

1. ✅ Implement SSO API in external system (see EXTERNAL_SYSTEM_SSO_SETUP.md)
2. ✅ Test the integration locally
3. ✅ Configure for production environment
4. ✅ Set up HTTPS for secure communication
5. ✅ Monitor SSO logs for issues

## Support

For detailed documentation, see:
- `README_SSO_INTEGRATION.md` - Complete SSO guide
- `EXTERNAL_SYSTEM_SSO_SETUP.md` - External system setup

## Logs

Monitor SSO activity:
```bash
tail -f storage/logs/laravel.log | grep SSO
```

Enable debug mode in `.env`:
```env
SSO_DEBUG_MODE=true
```

---

**Ready to go!** Follow the external system setup guide to complete the integration.
