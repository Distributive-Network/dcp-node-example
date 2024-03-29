# dcp-node-example

A Node.js example project that uses the Distributed Computer. It shows how to deploy a job to the Distributed Computer whilst
receiving events describing the current state of the job, processing results as
they are received, and so on.

Fork this github repository to get running quickly with the Distributed Computer on Node.js.

## Important files

| Filename                | Contents                                            |
| :---------------------- | --------------------------------------------------- |
| ~/.dcp/id.keystore      | Your identity proxy keystore                        |
| ~/.dcp/default.keystore | The 'bank account' key to use for paying for work   |

## Environment Variables

| Variable      | Behaviour                            |
| :------------ | ------------------------------------ |
| DCP_XHR_DEBUG | Shows network traffic on the console |
| DCP_XHR_PROXY | Set url of HTTP proxy server         |

### Getting Started

You will need Node 12 in your path. If you have an older version of NodeJS, use
`nvm`; see instructions in the FAQ. Additionally, you will need to copy your
identity and bank account keystores from the [portal](https://portal.distributed.computer/) of the Distributed Computer into the .dcp directory
inside your home directory. For more instructions on getting started, visit our [introductory guide here](https://docs.dcp.dev/getting-started).

Note: You will need to join our beta program to have permission to deploy jobs to the Distributed Computer. To do this, please fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLScj6g1PH7Nbejlj5XHrScvtBhTy-2A_l0A8sHMzzihQR79KYw/viewform)!

```shell
mkdir ~/.dcp
cp ~/Downloads/id.keystore ~/.dcp
cp ~/Downloads/default.keystore ~/.dcp
git clone git@github.com:Kings-Distributed-Systems/dcp-node-example.git
cd dcp-node-example
npm install
node app.js
```

### FAQ

#### Where do I get an identity proxy keystore?

Visit <https://portal.distributed.computer/>, then

- Portal
- First Devs
- Identity Keys
- Bottom right, green plus

#### Where do I get a bank account keystore?

Visit <https://portal.distributed.computer/>, then

- Wallet
- Accounts
- Default
- Click down-pointing chevron
- Click download

#### I don't want people using my identity proxy anymore. Can I revoke it?

Yes, remove it from the same screen in portal that you created it with, and it
will become invalid immediately.

#### I shared my bank account keystore and passphrase by accident. What should I do?

- Visit the portal
- Create a new bank account
- Transfer all your credits into the new account
- Remove the old account

#### What versions of NodeJS are supported?

We currently support Node 12 LTS.

If your operating system has a different version of Node.js, we suggest using
[nvm](https://github.com/nvm-sh/nvm#installing-and-updating) to run Node 12.

#### Can I use the node command line debugger with DCP Client?

No; the node debugger does not interoperate with programs that require you to
type passwords. You should use the GUI debuggers instead; node inspector or
vscode. If you are a die-hard CLI user, you can also try our experimental
debugger, [niim](https://github.com/wesgarland/niim). e.g.

```shell
$ sudo npm install --global niim

[sudo] password for wes:
/usr/bin/niim -> /usr/lib/node_modules/niim/niim
+ niim@1.11.6-g
updated 1 package in 2.371s

$ niim app.js
| niim: Node-Inspector IMproved - initialized
< Debugger listening on ws://127.0.0.1:34117/f1956572-a327-40b2-b70a-20a8b9961228
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.

Break on start in app.js:1
> 1
  2 /**
  3  * @file        app.js
niim> c
< new ready state: exec
Enter passphrase to unlock keystore 'hello-staging':

niim> ctty
< entering interactive terminal mode, ^C to exit (niim)
*****
new ready state: deploying
Enter passphrase to unlock keystore 'hello-staging': *****
new ready state: authorizeHold
new ready state: authorizeFeeStructure
 - Job accepted by scheduler, waiting for results
 - Job has id 0x7f8bd7b58e99f4dc5e9e26d4d898707f079368e484dfb0b52876458c9c7b7edfeb0cabb6c7cb8eb362c192c9d198d2d718e99aeb08f4d1edf5cfcf60ad1101029a
new ready state: deployed
<CONTROL-C>
| exited interactive terminal mode
niim>
```

#### Why does it take so long to get my results back?

Our scheduler is very busy!

We do prioritize work based on your _bid price_. This is the value per slice
assigned during to the call to `compute.exec()`.

We also have a slow-start period which is designed to measure the work you have
submitted, ensure that it is well-behaved, and so on. This means that we will
hold back your work until we get results back for 5-10 slices.
