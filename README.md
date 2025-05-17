# Blue-Collar-Dollar
A community based XRPL/Stellar token project where your input and ideas are requested. 
BCD is a publicly available 'private treasury token' equally available to us all on XRPL. 
BCD belongs to us all. literally!
Any transaction utilizing BCD token is a sovereign act of humanity
There are many new possibilities in this space
This webhook link is a new developement for the BCD project, This is a vital step in our collective journey
I have not invested the time nor had the opportunity to put guardrails in here at this time
Until that happens, respectful code writing cooperation is appreciated.
Please treat this space as you would your own, because... This public space belongs to each of us equally.
Until we put on more miles together, please be respectful and responsible in this space. 
It is actually connected to the GitHub platform where we co-create and develop real-life infrastructure for Blue-Collar-Dollar.
code writing is tricky for all of us. we all have lessons to learn from each other and knowledge to share.
This space is designed to allow all members of this server, a better opportunity to safely and securely learn code writing,
and develop an API utility for your BCD treasury, and also any XRPL token project where the issuer is connected to this discord server.
For every BCD in circulation there is a corresponding amount of XRP/XAU in one of the 3 treasury wallets today and forever.
Build your dreams here and the public will come!!!
invest in BCD and the public will thrive!!!

I am importing some basic tools and java script code for the ease of API development.

Some of these tools and code will not be very readable in preview text page. Those will need to be copy/pasted from code text page to your project files

# The following is a library of constants in js for transaction information

// *** Define HTML Form Fields **

const xls = document.getElementById("xls")

const tn = document.getElementById("tn")

const dn = document.getElementById("dn")

const standbyResultField = document.getElementById('standbyResultField');

const operationalResultsField = document.getElementById('operationalResultField');

const standbyAccountField = document.getElementById('standbyAccountField');

const standbyPubKeyField = document.getElementById('standbyPubKeyField');

const standbyPrivKeyField = document.getElementById('standbyPrivKeyField');

const standbyBalanceField = document.getElementById('standbyBalanceField');

const standbySeedField = document.getElementById('standbySeedField');

const operationalAccountField = document.getElementById('operationalAccountField');

const operationalPubKeyField = document.getElementById('operationalPubKeyField');

const operationalPrivKeyField = document.getElementById('operationalPrivKeyField');

const operationalSeedField = document.getElementById('operationalSeedField');

const operationalBalanceField = document.getElementById('operationalBalanceField');

# The following is for testnet/devnet java script AKA "tn/dn js" developers who want to develop and test drive their project in the BCD ecosystem

// ************* Get the Preferred Network **************
    function getNet() {
        let net
           if (tn.checked) net = "wss://s.altnet.rippletest.net:51233"
           if (dn.checked) net = "wss://s.devnet.rippletest.net:51233"
           return net
        } // End of getNet()

// *******************************************************
// ********** Get Accounts from Seeds ******************** 
// *******************************************************

      async function getAccountsFromSeeds() {
        let net = getNet()
        const client = new xrpl.Client(net)
        results = 'Connecting to ' + getNet() + '....'
        standbyResultField.value = results
        await client.connect()
        results += '\nConnected, finding wallets.\n'
        standbyResultField.value = results
      
        // -----------------------------------Find the test account wallets    
        var lines = seeds.value.split('\n');

        const standby_wallet = xrpl.Wallet.fromSeed(lines[0])
        const operational_wallet = xrpl.Wallet.fromSeed(lines[1])
      
        // -----------------------------------Get the current balance.
        const standby_balance = (await client.getXrpBalance(standby_wallet.address))
        const operational_balance = (await client.getXrpBalance(operational_wallet.address))  
        
        // ------------------Populate the fields for Standby and Operational accounts
        standbyAccountField.value = standby_wallet.address
        standbyPubKeyField.value = standby_wallet.publicKey
        standbyPrivKeyField.value = standby_wallet.privateKey
        standbySeedField.value = standby_wallet.seed
        standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
      
        operationalAccountField.value = operational_wallet.address
        operationalPubKeyField.value = operational_wallet.publicKey
        operationalPrivKeyField.value = operational_wallet.privateKey
        operationalSeedField.value = operational_wallet.seed
        operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
      
       client.disconnect()
            
      } // End of getAccountsFromSeeds()


// *******************************************************              
// ************* Get Account *****************************  
// *******************************************************
  
  async function getAccount(type) {
      let net = getNet()
      
      const client = new xrpl.Client(net)
        results = 'Connecting to ' + net + '....'
        
        // This uses the default faucet for Testnet/Devnet
        let faucetHost = null
        if(document.getElementById("xls").checked) {
            faucetHost = "faucet-nft.ripple.com"
        } 
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }
        await client.connect()
        
        results += '\nConnected, funding wallet.'
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }
        
        // -----------------------------------Create and fund a test account wallet
       const my_wallet = (await client.fundWallet(null, { faucetHost })).wallet
        
        results += '\nGot a wallet.'
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }       
      
        // -----------------------------------Get the current balance.
        const my_balance = (await client.getXrpBalance(my_wallet.address))  
        
        if (type == 'standby') {
          standbyAccountField.value = my_wallet.address
          standbyPubKeyField.value = my_wallet.publicKey
          standbyPrivKeyField.value = my_wallet.privateKey
          standbyBalanceField.value = 
              (await client.getXrpBalance(my_wallet.address))
          standbySeedField.value = my_wallet.seed
          results += '\nStandby account created.'
          standbyResultField.value = results
        } else {
          operationalAccountField.value = my_wallet.address
          operationalPubKeyField.value = my_wallet.publicKey
          operationalPrivKeyField.value = my_wallet.privateKey
          operationalSeedField.value = my_wallet.seed
          operationalBalanceField.value = 
              (await client.getXrpBalance(my_wallet.address))
          results += '\nOperational account created.'
          operationalResultField.value = results
        }
        // --------------- Capture the seeds for both accounts for ease of reload.
        seeds.value = standbySeedField.value + '\n' + operationalSeedField.value
        client.disconnect()
      } // End of getAccount()

# The following code is for creating seed accounts during tn/dn js test drives

// *******************************************************
// ********** Get Accounts from Seeds ******************** 
// *******************************************************

      async function getAccountsFromSeeds() {
        let net = getNet()
        const client = new xrpl.Client(net)
        results = 'Connecting to ' + getNet() + '....'
        standbyResultField.value = results
        await client.connect()
        results += '\nConnected, finding wallets.\n'
        standbyResultField.value = results
      
        // -----------------------------------Find the test account wallets    
        var lines = seeds.value.split('\n');

        const standby_wallet = xrpl.Wallet.fromSeed(lines[0])
        const operational_wallet = xrpl.Wallet.fromSeed(lines[1])
      
        // -----------------------------------Get the current balance.
        const standby_balance = (await client.getXrpBalance(standby_wallet.address))
        const operational_balance = (await client.getXrpBalance(operational_wallet.address))  
        
        // ------------------Populate the fields for Standby and Operational accounts
        standbyAccountField.value = standby_wallet.address
        standbyPubKeyField.value = standby_wallet.publicKey
        standbyPrivKeyField.value = standby_wallet.privateKey
        standbySeedField.value = standby_wallet.seed
        standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
      
        operationalAccountField.value = operational_wallet.address
        operationalPubKeyField.value = operational_wallet.publicKey
        operationalPrivKeyField.value = operational_wallet.privateKey
        operationalSeedField.value = operational_wallet.seed
        operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
      
       client.disconnect()
            
      } // End of getAccountsFromSeeds()

# The following is instructions for sending tokens (in this example its XRP) for tn/dn js test drives

// // *******************************************************
// ******************** Send XRP *************************
// *******************************************************

      async function sendXRP() {
      
        results  = "Connecting to the selected ledger.\n"
        standbyResultField.value = results
        let net = getNet()
        results = 'Connecting to ' + getNet() + '....'
        const client = new xrpl.Client(net)
        await client.connect()
      
        results  += "\nConnected. Sending XRP.\n"
        standbyResultField.value = results
      
        const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
        const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
        const sendAmount = standbyAmountField.value
        
        results += "\nstandby_wallet.address: = " + standby_wallet.address
        standbyResultField.value = results
      
        // ------------------------------------------------------- Prepare transaction
        // Note that the destination is hard coded.
        const prepared = await client.autofill({
          "TransactionType": "Payment",
          "Account": standby_wallet.address,
          "Amount": xrpl.xrpToDrops(sendAmount),
          "Destination": standbyDestinationField.value
        })
      
        // ------------------------------------------------ Sign prepared instructions
        const signed = standby_wallet.sign(prepared)
      
        // -------------------------------------------------------- Submit signed blob
        const tx = await client.submitAndWait(signed.tx_blob)
      
         results  += "\nBalance changes: " + 
            JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
         standbyResultField.value = results

        standbyBalanceField.value = 
          (await client.getXrpBalance(standby_wallet.address))
        operationalBalanceField.value = 
          (await client.getXrpBalance(operational_wallet.address))                 
        client.disconnect()
      
      } // End of sendXRP()

# The following is for the reciprical transaction for the purpose of settlement of the ledger

// *******************************************
// ****** Reciprocal Transactions ************

// ******************************************* 
// *****Send XRP from Operational account ****
// *******************************************
      
      async function oPsendXRP() {

        results  = "Connecting to the selected ledger.\n"
        operationalResultField.value = results
        let net = getNet()
        results = 'Connecting to ' + getNet() + '....'
        const client = new xrpl.Client(net)
        await client.connect()
      
        results  += "\nConnected. Sending XRP.\n"
        operationalResultField.value = results
      
        const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
        const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
        const sendAmount = operationalAmountField.value
        
        results += "\noperational_wallet.address: = " + operational_wallet.address
        operationalResultField.value = results
      
        // ------------------------------------------------------- Prepare transaction
        // Note that the destination is hard coded.
        const prepared = await client.autofill({
          "TransactionType": "Payment",
          "Account": operational_wallet.address,
          "Amount": xrpl.xrpToDrops(operationalAmountField.value),
          "Destination": operationalDestinationField.value
        })
      
        // ------------------------------------------------ Sign prepared instructions
        const signed = operational_wallet.sign(prepared)
      
        // -------------------------------------------------------- Submit signed blob
        const tx = await client.submitAndWait(signed.tx_blob)
      
         results  += "\nBalance changes: " + 
            JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
         operationalResultField.value = results
         
        standbyBalanceField.value = 
          (await client.getXrpBalance(standby_wallet.address))
        operationalBalanceField.value = 
          (await client.getXrpBalance(operational_wallet.address))                 
      
        client.disconnect()
      
      } // End of oPsendXRP()

# The following is HTML form preview: 1.get-accounts-send-xrp.html

<html>
  <head>
    <title>Token Test Harness</title>
    <link href='https://fonts.googleapis.com/css?family=Work Sans' rel='stylesheet'>
    <script src='https://unpkg.com/xrpl@2.2.3'></script>
    
    <script>
      if (typeof module !== "undefined") {
        const xrpl = require('xrpl')
      }
    </script>
    <style>
       body{font-family: "Work Sans", sans-serif;padding: 20px;background: #fafafa;font-size:.8em;}
       h1{font-weight: bold;}
       input, button {padding: 6px;margin-bottom: 8px;font-size:.8em;}
       button{font-weight: bold;font-family: "Work Sans", sans-serif;}
       td{vertical-align: top;padding-right:10px;}
    </style>
  </head>
    <form id="theForm">
      Choose your ledger instance:  
      <input type="radio" id="tn" name="server" value="wss://s.altnet.rippletest.net:51233" >
      <label for="testnet">Testnet</label>
      
      <input type="radio" id="dn" name="server" value="wss://s.devnet.rippletest.net:51233" checked>
      <label for="devnet">Devnet</label>
      <br/><br/>
      <button type="button" onClick="getAccountsFromSeeds()">Get Accounts From Seeds</button>
      <br/>
      <textarea id="seeds" cols="40" rows= "2">... You can use this later once you generate seeds</textarea>
      <br/><br/>

            <table>
              <tr valign="top">
                <td>
                  <button type="button" onClick="getAccount('standby')">Get New Standby Account</button>
                  <table>
                    <tr valign="top">
                      <td align="left">
                        Standby Account<br/>
                        <input type="text" id="standbyAccountField" size="30" />
                      </td>
                      <td></td>
                    </tr>
                    <tr>
                      <td align="left">
                        Public Key<br/>
                        <input type="text" id="standbyPubKeyField" size="30"></input>
                      </td>
                      <td align="left">
                        Private Key<br/>
                        <input type="text" id="standbyPrivKeyField" size="30"></input>
                      </td>
                    </tr>
                    <tr>
                      <td align="left">
                        Seed <br/>
                        <input type="text" id="standbySeedField" size="30"></input>
                        <br>
                      </td>
                      <td align="left">
                        XRP Balance <br/>
                        <input type="text" id="standbyBalanceField" size="30"></input>
                      </td>
                  </tr>
                  <tr>
                      <td align="left">
                        Amount<br/>
                        <input type="text" id="standbyAmountField" size="30"></input>
                      </td>
                      <td align="left">
                        Destination Account <br/>
                      <input type="text" id="standbyDestinationField" size="30"></input>
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <p align="right">
                          <button type="button" onClick="sendXRP()">Send XRP ↓</button>
                        </p>
                      </td>
                    </tr>
                  </table>
                </td>
                <td>
                  <textarea id="standbyResultField" cols="60" rows="20" ></textarea>
                </td>
              </tr>
            </table>

            <table>
              <tr valign="top">
                <td>
                  <button type="button" onClick="getAccount('operational')">Get New Operational Account</button>
                  <table>
                    <tr valign="top">
                      <td align="left">
                        Operational Account<br/> <input type="text" id="operationalAccountField" size="30" />
                      </td>
                      <td></td>
                    </tr>
                    <tr>
                      <td align="left">Public Key<br/>
                        <input type="text" id="operationalPubKeyField" size="30" />
                      </td>
                      <td align="left">
                        Private Key<br/>
                        <input type="text" id="operationalPrivKeyField" size="30"></input>
                      </td>
                    </tr>
                    <tr>
                      <td align="left">
                        Seed <br/>
                        <input type="text" id="operationalSeedField" size="30"></input>
                        <br>
                      </td>
                      <td align="left">
                        XRP Balance <br/>
                        <input type="text" id="operationalBalanceField" size="30" />
                      </td>
                  </tr>
                  <tr>
                      <td align="left">
                        Amount<br/>
                        <input type="text" id="operationalAmountField" size="30" />
                      </td>
                      <td align="left">
                        Destination Account <br/>
                      <input type="text" id="operationalDestinationField" size="30" />
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <p align="right">
                          <button type="button" onClick="oPsendXRP()">Send XRP ↑</button>
                        </p>
                      </td>
                    </tr>
                  </table>
                </td>
                <td>
                  <textarea id="operationalResultField" cols="60" rows="20" ></textarea>
                </td>
              </tr>
            </table>
    </form>
  </body>
  <script src='ripplex1-send-xrp.js' async></script>
</html>
