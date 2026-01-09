# TRAC Network dApp Test Suite

A comprehensive test page for the `window.tracnetwork` API integration in TAP Wallet Mobile.

## 🌐 Live Demo

**GitHub Pages**: [https://taipv00.github.io/windows-trac/](https://taipv00.github.io/windows-trac/)

## 🧪 What's Included

This test page provides interactive testing for all TRAC Network wallet integration methods:

### Read Methods (No Approval Required)
- ✅ `getAddress()` - Get TRAC address for active account
- ✅ `getBalance()` - Fetch TRAC balance
- ✅ `getPublicKey()` - Get public key for active account
- ✅ `getNetwork()` - Get current network (mainnet/testnet)

### Write Methods (Require User Approval)
- 🔐 `requestAccount()` - Request connection to TRAC wallet
- 🔐 `switchNetwork()` - Switch between mainnet/testnet
- ✍️ `signMessage()` - Sign a message with TRAC private key
- 💸 `sendTNK()` - Send TRAC tokens (TNK)

### Advanced Transaction Methods
- 🔨 `buildTracTx()` - Build transaction without broadcasting
- 🔏 `signTracTx()` - Sign pre-built transaction
- 📡 `pushTracTx()` - Broadcast signed transaction

## 🚀 How to Use

### In TAP Wallet Mobile

1. Open TAP Wallet app
2. Navigate to **dApp Browser**
3. Enter URL: `https://taipv00.github.io/windows-trac/`
4. Test all TRAC integration features

### For Development

```bash
# Clone repository
git clone git@github.com:taipv00/windows-trac.git
cd windows-trac

# Serve locally
python3 -m http.server 8080
# or
npx serve

# Open in browser
open http://localhost:8080
```

## 📱 Testing in Wallet

The page will automatically detect if `window.tracnetwork` is available:

- ✅ **Green status**: Provider detected, all buttons enabled
- ❌ **Red status**: Provider not found, please use TAP Wallet app

## 🎯 Test Scenarios

### Basic Flow
1. Click "Request Account" → Approve in wallet
2. Click "Get Address" → See your TRAC address
3. Click "Get Balance" → See your TNK balance

### Transaction Flow
1. Build transaction → Get transaction data
2. Sign transaction → Get signed payload
3. Push transaction → Broadcast to network

## 🔧 Integration Guide

### For dApp Developers

```javascript
// Check if TRAC provider is available
if (typeof window.tracnetwork !== 'undefined') {
  console.log('TRAC provider detected!');

  // Request account connection (shows approval modal)
  const address = await window.tracnetwork.requestAccount();

  // Get balance (no approval needed after connection)
  const balance = await window.tracnetwork.getBalance();

  // Sign message (shows approval modal)
  const signature = await window.tracnetwork.signMessage('Hello TRAC!');

  // Send tokens (shows approval modal)
  const result = await window.tracnetwork.sendTNK(null, recipientAddress, '0.001');
}
```

### Event Listener

```javascript
// Listen for provider initialization
window.addEventListener('tracnetwork#initialized', () => {
  console.log('TRAC provider initialized');
});
```

## 📋 API Reference

### `window.tracnetwork.requestAccount()`
Request connection to TRAC wallet. Shows approval modal.
- **Returns**: `Promise<string>` - TRAC address

### `window.tracnetwork.getAddress()`
Get TRAC address for active account.
- **Returns**: `Promise<string>` - TRAC address (bech32m format)

### `window.tracnetwork.getBalance(address?)`
Get TRAC balance.
- **Parameters**:
  - `address` (optional): TRAC address to query
- **Returns**: `Promise<string>` - Balance as string

### `window.tracnetwork.getPublicKey()`
Get public key for active account.
- **Returns**: `Promise<string>` - Public key as hex string

### `window.tracnetwork.getNetwork()`
Get current network.
- **Returns**: `Promise<string>` - 'livenet' or 'testnet'

### `window.tracnetwork.switchNetwork(network)`
Switch network. Shows approval modal.
- **Parameters**:
  - `network`: 'livenet' or 'testnet'
- **Returns**: `Promise<string>` - New network name

### `window.tracnetwork.signMessage(message)`
Sign message with TRAC private key. Shows approval modal.
- **Parameters**:
  - `message`: Message to sign
- **Returns**: `Promise<{signature: string, publicKey: string, address: string}>`

### `window.tracnetwork.sendTNK(from, to, amount)`
Send TRAC tokens. Shows approval modal.
- **Parameters**:
  - `from`: Optional sender address (uses active account if not provided)
  - `to`: Recipient TRAC address
  - `amount`: Amount to send (in TNK)
- **Returns**: `Promise<{txid: string}>`

### `window.tracnetwork.buildTracTx(params)`
Build transaction without broadcasting.
- **Parameters**:
  ```typescript
  {
    from?: string,    // Optional sender address
    to: string,       // Recipient address
    amount: string    // Amount in TNK
  }
  ```
- **Returns**: `Promise<object>` - Transaction data

### `window.tracnetwork.signTracTx(txData)`
Sign pre-built transaction. Shows approval modal.
- **Parameters**:
  - `txData`: Transaction data from `buildTracTx()`
- **Returns**: `Promise<string>` - Signed transaction payload

### `window.tracnetwork.pushTracTx(txPayload)`
Broadcast signed transaction.
- **Parameters**:
  - `txPayload`: Signed payload from `signTracTx()`
- **Returns**: `Promise<{txHash: string, success: boolean}>`

## 🛠️ Technical Details

### Provider Injection
The TRAC provider is injected via JavaScript:
- Namespace: `window.tracnetwork`
- Communication: `window.ReactNativeWebView.postMessage()`
- Event-based promise resolution

### Security
- Approval required for all sensitive operations
- Origin tracking for dApp connections
- User must explicitly approve each transaction

## 📝 Notes

- Always check if `window.tracnetwork` exists before using
- All approval methods show a modal in the wallet app
- Connection is required before using most methods
- Transactions are irreversible - double check before confirming

## 🐛 Issues & Feedback

Report issues at: [GitHub Issues](https://github.com/taipv00/windows-trac/issues)

## 📄 License

MIT License

---

Made with ❤️ by TAP Protocol Team
