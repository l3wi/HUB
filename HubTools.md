## Hub Tools

### Independent Confirmation of User Deposits

Independent confirmation of deposits may be helpful for analysis or problem solving.  Here are three ways to check whether a deposit was confirmed:

**via getBalances**

Check the balance of an address using the getBalances API call.  Continuously call getBalances with the deposit address to determine whether an address has a confirmed balance.

**via findTransactions**

Continuously call findTransactions on the deposit address. Once there is a transaction, get it’s bundle hash (via utils.transactionObjects) and search for all transactions with the same bundle hash. Then, get the correct tail transaction and call getInclusionState` on that transaction

**via zmq**

ZMQ allows you to subscribe and view transactions. A sample script using ZMQ is shown below along with sample output.

    let zmq = require('zeromq')
    let sock = zmq.socket('sub')
     
    sock.connect('tcp://zmq.devnet.iota.org:5556')
    sock.subscribe('tx')
    sock.subscribe('sn')
     
    sock.on('message', msg => {
      const data = msg.toString().split(' ') // Split to get topic & data
      switch (
            data[0] // Use index 0 to match topic
      ) {
            case 'tx':
            console.log(`I'm a TX!`, data)
            break
            case 'sn':
            console.log(`I'm a confirmed TX`, data)
            break
      }
    })
     
     

Sample Output:
    I'm a TX!

    [ 'tx',  'ZMHJERFCCJAJNIQBQ9ZGAFWGTDZBCOPKHPEAXFGSYQNJIDAJZHOAJWGYG9OCIFWCYESLTLTRNVRKLU999,
    'EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECY',
     '0',
     'RBVHA9999999999999999999999',
     '1532555817',
     '0',
     '1',
    '9WTKBVKJMW9ENHIOIJMKQSPDEMMHUNHVBZLLPPNVIQBOZVQJLSLYWUEBBXDKSPGBJIUFBGBZBUURVLJF',
    'YZSBYT9HSTGKWTNSMUUCN9IMBMGIWUCJZUFVQEF9TQVCVTIPNLPVOV9MARXOJGJUIPKOMUCMAPUNNJ99,
    'AGEKCZASCCGHBPSOIAHYPYGCZIGHRYKENFPQYYNJPAOITNJIFXMHFLQHXWDBYQBJSZKTFLQABMJEOG999',
     '1532555817540', 'RBVHA9999999999999999999999' ]
     
    I'm a confirmed TX

    [ 'sn',  '685304',
    'AGEKCZASCCGHBPSOIAHYPYGCZIGHRYKENFPQYYNJPAOITNJIFXMHFLQHXWDBYQBJSZKTFLQABMJEOG999',
    'EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECY',
    'BLCKFZJYCYVUTWFWSGV9EEQCWCRJSIUZHUCEUDLAKFXDFWSSOBBICFYTFXLAODQRHBUJBNXEIOVCVS99,
    'HXDFFUGKOPZNRSSOOU9AZDIRVOTFMVSKJ9WECXGNTRKEPLCW9EX99KKONIDBAVHHNIFRYEAUSYQBMM9,
    'XVXCEJBNQBDHQDNFJRMACFHXVQTLFCWQHTSRZMUBBDPY9AWYVCHPAOLPVDFBDUGARQRX9ZLBWNOEG']

### Monitoring Confirmation & Reattachment

Withdrawal transactions can be monitored and users notified when a withdrawal transaction has been confirmed. The following example utilizes getLatestInclusion() to check for confirmed transactions.

JavaScript

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


Withdrawal transaction with tail hash:
TAOIAXACMCWFYETKBHOTHNLNAZOWFORNAOKH9EIXVESLWBULGBKYRKUJLAQPKKNLISL9UHOXQESNLO999 confirmed

Screen Capture showing the confirmed bundle

![](images/TXconfirm.png?raw=true)

