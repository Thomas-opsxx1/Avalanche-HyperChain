# Avalanche HyperSDK

In this project, we explore the power of the HyperSDK, which enables the creation of fully customizable virtual machines (VMs) for building tailored blockchains. While pre-built subnets offer convenience, they may not always meet specific business requirements. HyperSDK addresses this limitation by allowing startups and organizations to design and deploy blockchain solutions with complete control over the functionality, rules, and architecture.

With HyperSDK, you can create blockchains for use cases such as token creation, asset transfers, or even implementing a traditional order book for asset trading. This flexibility ensures that the blockchain aligns perfectly with the unique needs of your organization, offering a competitive advantage in the market.

By harnessing HyperSDK, your startup gains the ability to craft a bespoke blockchain solution that fits your users’ demands, positioning you with a powerful, customizable tool to navigate today's fast-evolving digital landscape.

## Project Challenge 

**Challenge:** Your startup has identified a need to create a custom virtual machine to enable users to mint and transfer tokens. The HyperSDK provides a powerful solution for this task by allowing you to build a custom blockchain tailored to your specific needs. With the HyperSDK, you can define the rules and functionality of your chain, including the ability to create and transfer tokens and manage order books for trading assets.

## HyperChain

**Feature: Trade Any Two Tokens**

Tokenvm supports on-chain token trading, allowing users to create offers and trade tokens seamlessly. It features an in-memory order book optimized for fast, efficient transactions with support for partial fills and refunds.

---

**Feature: Avalanche Warp Support**

Tokenvm leverages Avalanche Warp Messaging (AWM) for secure cross-subnet token transfers, ensuring assets move safely between tokenvms without needing trusted relayers or bridges.

---

**Feature: Sandwich Resistance**

Tokenvm prevents front-running by ensuring all trades with an order occur at the same price. The chain cannot manipulate transactions, preserving fair trading conditions.

## HyperChain Registry Introduction 

```go
// When registering new actions, ALWAYS make sure to append at the end.
		consts.ActionRegistry.Register(&actions.Transfer{}, actions.UnmarshalTransfer, false),

		// TODO: register action: actions.CreateAsset [Modified]
		consts.ActionRegistry.Register(&actions.CreateAsset{}, actions.UnmarshalCreateAsset, false),
		// TODO: register action: actions.MintAsset [Modified]
		consts.ActionRegistry.Register(&actions.MintAsset{}, actions.UnmarshalMintAsset, false),
		

		consts.ActionRegistry.Register(&actions.BurnAsset{}, actions.UnmarshalBurnAsset, false),
		consts.ActionRegistry.Register(&actions.ModifyAsset{}, actions.UnmarshalModifyAsset, false),

		consts.ActionRegistry.Register(&actions.CreateOrder{}, actions.UnmarshalCreateOrder, false),
		consts.ActionRegistry.Register(&actions.FillOrder{}, actions.UnmarshalFillOrder, false),
		consts.ActionRegistry.Register(&actions.CloseOrder{}, actions.UnmarshalCloseOrder, false),

		consts.ActionRegistry.Register(&actions.ImportAsset{}, actions.UnmarshalImportAsset, true),
		consts.ActionRegistry.Register(&actions.ExportAsset{}, actions.UnmarshalExportAsset, false),
```

**Registry Description:**

The registry is responsible for registering all actions that can be executed within the system. Each action must be appended to the registry in a specific order to ensure proper execution. The registration process links each action to its corresponding unmarshalling function, which defines how the action is processed when received in serialized form.

For example:
- **Transfer**: The `Transfer` action is registered to handle token transfers, using `UnmarshalTransfer` to decode the action.
- **CreateAsset** and **MintAsset**: These actions are responsible for creating and minting tokens, registered with their respective unmarshal functions.
- **BurnAsset** and **ModifyAsset**: These allow token burning and modification of asset properties.
- **CreateOrder**, **FillOrder**, and **CloseOrder**: Registered to handle on-chain trading orders, allowing users to create, fill, and close trades.
- **ImportAsset** and **ExportAsset**: Enable cross-subnet asset transfers, with special treatment for importing actions (third parameter `true`).

Each action registration follows a pattern of linking the action with its decoding function, ensuring the system knows how to handle each operation effectively.

### Steps to Initiate the Hyperchain:

1. **Normalize Dependencies:**
   Inside your project folder, run the following command to tidy up and normalize all the dependencies:
   ```bash
   go mod tidy
   ```

2. **Configure Project Constants:**
   Open the `consts/consts.go` file and fill in the necessary values to configure your project constants.

3. **Register Actions:**
   In `registry/registry.go`, register the missing actions for **CreateAsset** and **MintAsset**.

4. **Run VM Locally:**
   Ensure Go is in your system's PATH. You can set it with:
   ```bash
   export PATH=$PATH:$(go env GOPATH)/bin
   ```
   If the above path doesn’t work, try:
   ```bash
   export PATH=$PATH:/usr/local/go/bin
   ```

5. **Run the Local VM:**
   To run the VM locally, use the following command:
   ```bash
   MODE="run-single" ./scripts/run.sh
   ```

6. **Build the VM:**
   Build the VM using:
   ```bash
   ./scripts/build.sh
   ```
   If you encounter a "permission denied" error, use the bash command:
   ```bash
   bash ./scripts/run.sh
   ```

7. **Load Demo Private Key:**
   Import the demo private key and chain setup by running:
   ```bash
   ./build/token-cli key import demo.pk
   ./build/token-cli chain import-anr
   ```

### Conclusion

By following the above steps, you should now have a fully functional Hyperchain running locally, with custom assets and actions like **CreateAsset** and **MintAsset** successfully registered. This setup allows you to explore the capabilities of the HyperSDK and begin experimenting with your custom blockchain features.

If you encounter any issues or need further assistance, make sure to check the project documentation or reach out to the support community.

Now that your Hyperchain is up and running, you can start building and integrating advanced blockchain solutions tailored to your specific needs. Happy coding!
