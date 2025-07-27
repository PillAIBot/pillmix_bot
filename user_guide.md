# PILL Mixer Bot - User Guide

Welcome to the PILL Mixer Bot (@pillmix_bot)! This comprehensive guide will walk you through every step of using the bot to mix and distribute your PILL tokens securely and privately.

## 🚀 Quick Start

1. **Start the bot**: Send `/start` to get your main wallet
2. **Fund your wallet**: Send SOL and PILL to your main wallet address
3. **Mix your tokens**: Use `/mixto [number]` to create mixed wallets
4. **Backup keys**: Use `/getmixkeys [batch]` to save your private keys
5. **Register external wallets**: Use `/regpub [batch] [addresses...]` to add destination wallets
6. **Transfer everything**: Use `/sendpub [batch]` to send all funds to external wallets

## 📋 Complete Command Reference

### 🔧 Wallet Management Commands

#### `/start`
- **Purpose**: Creates your main wallet and initializes the bot
- **Usage**: `/start`
- **What it does**: 
  - Generates a new Solana wallet if you don't have one
  - Shows your existing wallet if you already have one
  - Provides your main wallet address for funding

#### `/address`
- **Purpose**: Shows your main wallet address
- **Usage**: `/address`
- **Use case**: When you need to copy your wallet address for sending funds

#### `/private`
- **Purpose**: Shows your main wallet private key (sent privately)
- **Usage**: `/private`
- **⚠️ Security**: This message is sent privately and deleted after viewing
- **Use case**: For backing up your main wallet private key

#### `/balance`
- **Purpose**: Check your current SOL and PILL balances
- **Usage**: `/balance`
- **Shows**: 
  - SOL balance (for transaction fees)
  - PILL token balance
  - Whether you have enough funds for mixing

#### `/reset`
- **Purpose**: Creates a completely new main wallet
- **Usage**: `/reset`
- **⚠️ Warning**: This will generate a new wallet - make sure to backup your old one first!

### 🔄 Mixing Commands

#### `/mixto [count]`
- **Purpose**: Start the mixing process to create multiple sub-wallets
- **Usage**: `/mixto 10` (creates 10 mixed wallets)
- **Range**: 2-100 wallets
- **Requirements**: 
  - Minimum 500,000 PILL tokens
  - Sufficient SOL for fees (see breakdown below)
- **SOL Requirements** (operational funding, not lost fees):
  - **Layer 1 Funding**: 0.06-0.09 SOL per wallet (gets transferred forward)
  - **Transaction Fees**: ~0.01 SOL per transaction (actual network fees)
  - **Buffer**: 0.1 SOL safety buffer
  - **Important**: Most SOL is transferred to your final wallets, not lost as fees
  - **Example**: 5 wallets ≈ 0.55 SOL, 10 wallets ≈ 1.0 SOL, 20 wallets ≈ 1.4 SOL
- **Process**: Creates a new batch and mixes your PILL through 3 layers
- **Example**: `/mixto 5` creates 5 mixed wallets in batch #1

#### `/getmixcount`
- **Purpose**: Shows how many mixing batches you've created
- **Usage**: `/getmixcount`
- **Shows**: Total number of completed mixes

#### `/getmixkeys [batch_number]`
- **Purpose**: Get private keys for a specific batch
- **Usage**: `/getmixkeys 1` (gets keys for batch #1)
- **⚠️ Important**: Save these keys securely - they contain your mixed funds!
- **Format**: Shows public address and private key for each wallet in the batch

#### `/mixdelkeys [batch_number]`
- **Purpose**: Delete private keys for a specific batch from the database
- **Usage**: `/mixdelkeys 1`
- **⚠️ Warning**: Only use this after you've safely backed up your keys!
- **Confirmation**: Requires typing "yes" to confirm deletion

### 🎯 External Distribution Commands

#### `/regpub [batch_number] [address1] [address2] ...`
- **Purpose**: Register external wallet addresses where you want to receive your mixed funds
- **Usage**: `/regpub 1 ABC123... DEF456... GHI789...`
- **Limits**: 
  - Up to 10 addresses per command
  - Must register exactly as many addresses as wallets in the batch
- **Validation**: 
  - Checks if addresses are valid Solana addresses
  - Prevents duplicate addresses (across all users)
  - Prevents adding the same address twice
- **Example**: If you created 5 wallets with `/mixto 5`, you need to register exactly 5 external addresses

#### `/sendpub [batch_number]`
- **Purpose**: Transfer all PILL and SOL from your mixed wallets to your registered external addresses
- **Usage**: `/sendpub 1`
- **Requirements**: All external addresses must be registered first
- **Process**: 
  - Transfers maximum PILL from each sub-wallet to corresponding external address
  - Transfers maximum SOL (keeping minimal amount for transaction fees)
  - Marks the batch as complete when finished
- **⚠️ Final Step**: After this command, no further operations are possible on this batch

#### `/outreset [batch_number]`
- **Purpose**: Delete registered external addresses if you haven't transferred yet
- **Usage**: `/outreset 1`
- **When to use**: If you made a mistake registering addresses and want to start over
- **Limitation**: Only works if `/sendpub` hasn't been executed yet

#### `/status` (Admin Only)
- **Purpose**: Shows the status of all batches and system information
- **Usage**: `/status`
- **Access**: Restricted to bot administrators only
- **Note**: Regular users should use the individual commands to check their batch progress
- **Shows for each batch**:
  - Number of sub-wallets created
  - Number of external addresses registered
  - Number of completed transfers
  - Next recommended action

### 🔧 Admin Commands (Restricted Access)

The following commands are restricted to bot administrators only:

#### `/health` (Admin Only)
- **Purpose**: Shows comprehensive system health status
- **Usage**: `/health`
- **Access**: Restricted to configured admin users only
- **Shows**: Database health, RPC status, system resources, encryption status

#### `/backup` (Admin Only)
- **Purpose**: Creates manual database and system backups
- **Usage**: `/backup`
- **Access**: Restricted to configured admin users only
- **Function**: Creates compressed backups of database, configuration, and logs

#### `/status` (Admin Only)
- **Purpose**: Shows system-wide batch status and monitoring information
- **Usage**: `/status`
- **Access**: Restricted to configured admin users only
- **Note**: This provides administrative oversight of all system batches

**Note**: Regular users do not have access to these commands. If you need to check your batch progress, use the individual commands like `/getmixcount` and check your batch status through the external distribution commands.

## 📖 Step-by-Step Tutorial

### Scenario: Splitting 1,000,000 PILL into 10 separate wallets

#### Step 1: Initialize Your Wallet
```
Send: /start
Bot Response: Shows your main wallet address
```

#### Step 2: Fund Your Main Wallet
- Send **1 SOL** (for transaction fees) to your main wallet address
- Send **1,000,000 PILL** tokens to your main wallet address
- Verify with `/balance`

#### Step 3: Create Mixed Wallets
```
Send: /mixto 10
Bot Response: Creates batch #1 with 10 mixed sub-wallets
Process: Your PILL is mixed through 3 layers and distributed to 10 wallets
```

#### Step 4: Backup Your Keys
```
Send: /getmixkeys 1
Bot Response: Shows 10 wallet addresses and their private keys
Action: Save these keys securely (write them down or store safely)
```

#### Step 5: Prepare External Wallets
- Create 10 new wallets in Phantom (or your preferred wallet)
- Copy the 10 public addresses

#### Step 6: Register External Addresses
```
Send: /regpub 1 [paste your 10 Phantom addresses here]
Bot Response: Confirms how many addresses were added successfully
```

#### Step 7: Transfer to External Wallets
```
Send: /sendpub 1
Bot Response: Transfers all PILL and SOL to your 10 Phantom wallets
Result: Each Phantom wallet receives approximately 100,000 PILL + remaining SOL
```

#### Step 8: Verify Completion
```
Send: /getmixcount
Bot Response: Shows you have completed 1 batch

# You can also try the regpub command to see if the batch is complete:
Send: /regpub 1
Bot Response: If complete, will show "Cannot reset batch #1 - transfers have already been completed"
```

## ⚠️ Important Safety Guidelines

### 🔐 Security Best Practices
- **Never share your private keys** with anyone
- **Always backup your keys** before using `/mixdelkeys`
- **Double-check external addresses** before registering them
- **Use the bot only in private messages** (it won't work in groups)

### 💰 Financial Considerations
- **Minimum PILL**: You need at least 500,000 PILL tokens to start mixing
- **SOL requirements**: Budget approximately 0.09 SOL per wallet + small transaction fees (most gets transferred to your final wallets)
- **Complete transfer**: All funds will be moved to external wallets - nothing stays in sub-wallets

### 🚫 Common Mistakes to Avoid
- Don't register more addresses than you have sub-wallets
- Don't use the same external address twice
- Don't forget to backup your keys before deleting them
- Don't try to use commands on completed batches

## 🔍 Understanding Batch Status

### Batch Lifecycle
1. **Created**: Batch exists with sub-wallets, but no external addresses registered
2. **Partially Registered**: Some external addresses registered, but not all
3. **Fully Registered**: All external addresses registered, ready for transfer
4. **Complete**: All transfers completed, no further operations possible

### How to Check Your Batch Progress

Since `/status` is now admin-only, here's how to check your progress:

1. **Check total batches**: Use `/getmixcount` to see how many batches you've created
2. **Verify batch exists**: Use `/getmixkeys [batch_number]` to confirm a batch exists
3. **Check registration status**: Try `/regpub [batch_number]` without addresses to see current status
4. **Check transfer status**: Try `/sendpub [batch_number]` to see if transfers are needed or complete

### Example Progress Check
```
# Check how many batches you have
Send: /getmixcount
Response: You have completed 2 mixes

# Check if batch 1 is complete
Send: /sendpub 1
Response: "Batch #1 is already completed" (if done) or transfer process starts (if ready)

# Check if you need to register more addresses for batch 2
Send: /regpub 2
Response: "You need to register 2 more addresses before sending" (if incomplete)
```

## 🆘 Troubleshooting

### "Batch does not exist"
- Check your batch numbers with `/getmixcount`
- Make sure you've completed the mixing process with `/mixto`
- Try using `/getmixkeys [batch_number]` to verify the batch exists

### "Invalid wallet address format"
- Ensure you're using valid Solana wallet addresses
- Check for typos in the address

### "Already added by another user"
- Someone else has already registered this address
- Use a different external wallet address

### "You need to register X more addresses"
- You haven't registered enough external addresses
- Use `/regpub [batch] [more addresses...]` to add the remaining ones

### "Insufficient SOL for fees"
- Add more SOL to your main wallet
- You need approximately 0.09 SOL per wallet + small transaction fees (most gets transferred forward)

## 📞 Support

If you encounter issues:
1. Check this user guide first
2. Use `/getmixcount` to see how many batches you have
3. Use `/getmixkeys [batch_number]` to verify your batches exist
4. Ensure you're following the correct sequence of commands
5. Verify you have sufficient funds (SOL and PILL)

## 🎯 Pro Tips

- **Start small**: Try with 2-3 wallets first to understand the process
- **Organize your external wallets**: Label them clearly in your wallet app
- **Keep records**: Note down which batch corresponds to which external wallets
- **Plan your fees**: Always have extra SOL for unexpected transaction costs
- **Check your progress regularly**: Use `/getmixcount` and try the next step commands to understand where you are in the process

---

**Remember**: The PILL Mixer Bot is designed to give you complete control over your token distribution while maintaining privacy through the mixing process. Take your time, follow the steps carefully, and always prioritize the security of your private keys!
