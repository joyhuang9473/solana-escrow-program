### Environment Setup
1. Install Rust from https://rustup.rs/
2. Install Solana from https://docs.solana.com/cli/install-solana-cli-tools#use-solanas-install-tool

### Compile, Depoly and Setup

1.Start the validator

```
$ solana-test-validator
...
```

2.Compile and deploy the program

```
$ cargo build-bpf
...

$ solana program deploy target/deploy/bpf_program_template.so
Program Id: AbZPvDpwh53TGR8if3xx9ZCJFz53DkZ7N6ToZ7YRyZi5
```

3.Generate Keypairs

```
$ solana-keygen new -o keys/id.json
...

$ solana-keygen new -o keys/alice.json
...

$ solana-keygen new -o keys/bob.json
...
```

4.Retrieve the address for all of them and paste it to the *_pub.json files accordingly.

For example:

```
$ solana address -k keys/id.json
CFKr2MSCHHRyzCabdcomrc6S48zeWTcMb1kuum26yeMA

// keys/id_pub.json

"CFKr2MSCHHRyzCabdcomrc6S48zeWTcMb1kuum26yeMA"
```

5.Update the programId to be looked up

```
// keys/program_pub.json

"AbZPvDpwh53TGR8if3xx9ZCJFz53DkZ7N6ToZ7YRyZi5"
```

6.Fund the transaction payer in advance

```
$ solana transfer CFKr2MSCHHRyzCabdcomrc6S48zeWTcMb1kuum26yeMA 100 --allow-unfunded-recipient
```

### Run the Client Code

```
$ ts-node ts/setup.ts
Requesting SOL for Alice...
Requesting SOL for Bob...
Requesting SOL for Client...
Creating Mint X...
Creating Alice TokenAccount for X...
Creating Bob TokenAccount for X...
Sending 50X to Alice's X TokenAccount...
Creating Mint Y...
Creating Alice TokenAccount for Y...
Creating Bob TokenAccount for Y...
Sending 50Y to Bob's Y TokenAccount...
✨Setup complete✨

┌─────────┬───────────────────────┬───────────────────────┬─────────────────────┬─────────────────────┐
│ (index) │ Alice Token Account X │ Alice Token Account Y │ Bob Token Account X │ Bob Token Account Y │
├─────────┼───────────────────────┼───────────────────────┼─────────────────────┼─────────────────────┤
│    0    │          50           │           0           │          0          │         50          │
└─────────┴───────────────────────┴───────────────────────┴─────────────────────┴─────────────────────┘

$ ts-node ts/alice.ts
Sending Alice's transaction...
✨Escrow successfully initialized. Alice is offering 5X for 3Y✨

┌─────────┬───────────────────────┬───────────────────────┬─────────────────────┬─────────────────────┬───────────────────────────┐
│ (index) │ Alice Token Account X │ Alice Token Account Y │ Bob Token Account X │ Bob Token Account Y │ Temporary Token Account X │
├─────────┼───────────────────────┼───────────────────────┼─────────────────────┼─────────────────────┼───────────────────────────┤
│    0    │          45           │           0           │          0          │         50          │             5             │
└─────────┴───────────────────────┴───────────────────────┴─────────────────────┴─────────────────────┴───────────────────────────┘


$ ts-node ts/bob.ts
Sending Bob's transaction...
✨Trade successfully executed. All temporary accounts closed✨

┌─────────┬───────────────────────┬───────────────────────┬─────────────────────┬─────────────────────┐
│ (index) │ Alice Token Account X │ Alice Token Account Y │ Bob Token Account X │ Bob Token Account Y │
├─────────┼───────────────────────┼───────────────────────┼─────────────────────┼─────────────────────┤
│    0    │          45           │           3           │          5          │         47          │
└─────────┴───────────────────────┴───────────────────────┴─────────────────────┴─────────────────────┘
```