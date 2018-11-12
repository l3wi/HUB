## Hub Tools

### Monitoring Confirmation

Withdrawal transactions can be monitored and users notified when a withdrawal transaction has been confirmed. The following example utilizes getLatestInclusion() to check for confirmed transactions.

JavaScript
```
const IOTA = require("iota.lib.js")
const iota = new IOTA({provider: "https://nodes.testnet.iota.org:443"})
const remoteCurl = require('@iota/curl-remote')

withdrawalHashes = [ 'TAOIAXACMCWFYETKBHOTHNLNAZOWFORNAOKH9EIXVESLWBULGBKYRKUJLAQPKKNLISL9UHOXQESNLO999' ]

// Check inclusion states
iota.api.getLatestInclusion(withdrawalHashes, (err, states) => {
if (err) {
    console.log("Error: ", err)
    return
} else {
        console.log("States: ", states)
}
    states.forEach((confirmed, i) => {
        if (confirmed) {
        console.log('Withdrawal transaction with tail hash:', withdrawalHashes[i], 'confirmed')
                // Remove transaction hash from pending withdrawals list...
        }
})
})
```

**Withdrawal transaction with tail hash**

```TAOIAXACMCWFYETKBHOTHNLNAZOWFORNAOKH9EIXVESLWBULGBKYRKUJLAQPKKNLISL9UHOXQESNLO999 confirmed```


Screen Capture showing the confirmed bundle

![screen capture showing the confirmed bundle for the withdrawal transaction tail hash in the text](images/TXconfirm.png?raw=true)
