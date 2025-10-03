# SignalWire Outbound Caller

A simple HTML/JavaScript application for making outbound calls using SignalWire's Call Fabric SDK.

## Features

- **Automatic SWML Resource Management**: Automatically checks for and creates the required SWML script resource
- **Guest Token Authentication**: Automatically creates guest tokens with targeted permissions
- **Outbound Calling**: Place calls to any phone number via SWML resources
- **Real-time Status**: Live call status updates and error handling
- **Bootstrap UI**: Clean, responsive interface
- **Phone Number Formatting**: Automatic E.164 formatting for phone numbers

## Usage

1. **Open the Application**: Open `outbound-caller.html` in a web browser
2. **Configure Credentials**:
   - **Project ID**: Your SignalWire project ID
   - **Space Name**: Your SignalWire space (e.g., `yourspace.signalwire.com`)
   - **API Token**: Your SignalWire authentication token
3. **Connect**: Click "Connect to SignalWire" to authenticate and initialize the client
4. **Make Calls**: Enter a phone number and click "Call"

## Configuration Options

### Required Fields
- **Project ID**: Found in your SignalWire console
- **Space Name**: Your SignalWire space domain
- **API Token**: Your SignalWire authentication token
- **Phone Number**: Target phone number (auto-formatted to E.164)


## API Integration

The application uses SignalWire's REST API to:

1. **Check Resources**: `GET /api/fabric/resources`
   - Checks if "call pstn html" SWML script resource exists
2. **Create SWML Resource**: `POST /api/fabric/resources/swml_scripts`
   - Creates the SWML script resource if it doesn't exist
3. **Get Resource Addresses**: `GET /api/fabric/resources/swml_scripts/{id}/addresses`
   - Retrieves the address UUID for the SWML resource
4. **Create Guest Token**: `POST /api/fabric/guests/tokens`
   - Creates a temporary authentication token with specific address permissions
   - Uses Basic Auth with Project ID and API Token
5. **Initialize Client**: Uses the SignalWire JavaScript SDK
   - Connects using project ID and guest token
6. **Make Calls**: Uses the `client.dial()` method
   - Passes user variables to SWML script
   - Handles call state changes and events

## SWML Integration

The application works with SWML (SignalWire Markup Language) scripts that handle call logic. The automatically created SWML script receives:

- `destination_number`: The formatted phone number to call

The SWML script performs these actions:
1. Records the call in WAV format
2. Connects to the specified destination number

## File Structure

```
outbound-caller.html      # Main HTML interface
outbound-caller.js        # JavaScript application logic
call-pstn-html.json       # SWML script definition (auto-deployed)
README-outbound-caller.md # This documentation
```

## Error Handling

The application includes comprehensive error handling for:
- Authentication failures
- Network connectivity issues
- Invalid phone numbers
- Call setup errors
- API rate limits

## Security Notes

- Guest tokens are temporary and scoped to specific operations
- Credentials are only stored in browser memory (not localStorage)
- All API calls use HTTPS encryption
- No sensitive data is logged to console in production

## Browser Compatibility

- Modern browsers with WebRTC support
- HTTPS required for WebRTC functionality
- JavaScript ES6+ features used

## Troubleshooting

1. **Connection Issues**: Verify space name, project ID, and API token
2. **Resource Creation Failures**: Ensure your API token has permissions to create SWML resources
3. **Call Failures**: The SWML resource should be created automatically - check connection status messages
4. **Audio Issues**: Ensure browser has microphone permissions
5. **CORS Errors**: Serve files from a web server (not file://)

## Based On

This application combines concepts from:
- The `new.js` example showing SignalWire client initialization and calling
- The existing `webphone/` Flask application structure
- SignalWire's official JavaScript SDK documentation
