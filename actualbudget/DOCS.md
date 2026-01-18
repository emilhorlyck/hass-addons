# Actual Budget Add-on Documentation

## Accessing Actual Budget

### Recommended: Via Home Assistant Ingress

The recommended way to access Actual Budget is through Home Assistant's ingress feature:

1. **Open Web UI button**: Click the "Open Web UI" button on the add-on page
2. **Sidebar**: If you have the add-on shown in your sidebar, click on it

This method routes traffic through Home Assistant's secure connection and ensures full compatibility with Actual Budget's features.

### Alternative: Direct Port Access

You can also access Actual Budget directly at `http://<your-home-assistant-ip>:5006`. However, this method may cause issues (see Troubleshooting below).

## Troubleshooting

### SharedArrayBuffer Error

If you see an error like:

> SharedArrayBuffer is not defined

or

> This browser or environment is not supported

This occurs when accessing Actual Budget over plain HTTP (not HTTPS). Actual Budget requires `SharedArrayBuffer`, which browsers only enable in secure contexts.

**Solution**: Access Actual Budget through Home Assistant ingress (click "Open Web UI" or use the sidebar link). This routes through Home Assistant's connection and provides the required security headers.

**Why direct port access fails**: When you access `http://<ip>:5006` directly, the browser blocks `SharedArrayBuffer` because the connection is not secure. The ingress method works because Home Assistant handles the security requirements.

### Budget Sync Issues

If you experience sync issues:

1. Ensure the add-on has restarted after any configuration changes
2. Check that your data directory has proper permissions
3. Review the add-on logs for any error messages

## Data Storage

Your budget data is stored in `/data` within the add-on container, which maps to persistent storage managed by Home Assistant. This data persists across add-on restarts and updates.

## More Information

- [Actual Budget Documentation](https://actualbudget.org/docs/)
- [Actual Budget GitHub](https://github.com/actualbudget/actual)
