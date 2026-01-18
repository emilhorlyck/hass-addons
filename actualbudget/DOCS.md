# Actual Budget Add-on Documentation

## Accessing Actual Budget

### Recommended: Via Home Assistant Ingress

The recommended way to access Actual Budget is through Home Assistant's ingress feature:

1. **Open Web UI button**: Click the "Open Web UI" button on the add-on page
2. **Sidebar**: If you have the add-on shown in your sidebar, click on it

This method routes traffic through Home Assistant's secure connection and ensures full compatibility with Actual Budget's features, including SharedArrayBuffer support required for the app to function properly.

### Alternative: Direct Port Access

You can also access Actual Budget directly at `http://<your-home-assistant-ip>:5006`. Note that some browser features (like SharedArrayBuffer) may not work over plain HTTP - use HTTPS or the ingress method for full functionality.

## How It Works

This add-on wraps the official `actualbudget/actual-server` Docker image with an nginx reverse proxy that:
- Handles Home Assistant ingress routing
- Rewrites absolute paths to work correctly through the ingress subpath
- Passes through the required security headers (COOP/COEP) for SharedArrayBuffer

## Data Storage

Your budget data is stored persistently and survives add-on updates and restarts.

## Troubleshooting

### Blank Screen via Ingress

If you see a blank screen when accessing via ingress, check the add-on logs for nginx errors. The lua-based path rewriting should handle most cases, but some edge cases may require adjustments.

### SharedArrayBuffer Error via Direct Access

If you see "SharedArrayBuffer is not defined" when accessing directly via `http://<ip>:5006`, this is expected on non-HTTPS connections. Use the ingress method (Open Web UI) instead, or set up an external HTTPS reverse proxy.

### API Errors

If you see API-related errors, ensure the add-on has fully started (check logs for "Listening on :::5007").

## More Information

- [Actual Budget Documentation](https://actualbudget.org/docs/)
- [Actual Budget GitHub](https://github.com/actualbudget/actual)
