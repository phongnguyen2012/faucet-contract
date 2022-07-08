## faucet-contract

  <sup>
  export MAIN_ACCOUNT=phongnguyen2022.testnet
  export NEAR_ENV=testnet
  export CONTRACT_FAUCET_ID=faucet.$MAIN_ACCOUNT
  export CONTRACT_FT_ID=ft.$MAIN_ACCOUNT
  export ONE_YOCTO=0.000000000000000000000005
  export ACCOUNT_TEST1=test1.$MAIN_ACCOUNT
  export ACCOUNT_TEST2=test2.$MAIN_ACCOUNT
  export GAS=300000000000000
</sup>
  echo "################### DELETE ACCOUNT ###################"
  <sup>
  near delete $CONTRACT_FAUCET_ID $MAIN_ACCOUNT
  near delete $ACCOUNT_TEST1 $MAIN_ACCOUNT
  near delete $ACCOUNT_TEST2 $MAIN_ACCOUNT
</sup>
   echo "################### CREATE ACCOUNT ###################"
  <sup>
   near create-account $CONTRACT_FT_ID --masterAccount $MAIN_ACCOUNT --initialBalance 2
   near create-account $ACCOUNT_TEST1 --masterAccount $MAIN_ACCOUNT --initialBalance 2
   near create-account $ACCOUNT_TEST2 --masterAccount $MAIN_ACCOUNT --initialBalance 2
  </sup>
<sup>
   #### 1. Deploy:
   near deploy --wasmFile out/faucetcontract.wasm --accountId $CONTRACT_FAUCET_ID

   #### 2. Init contract: with max_share: 10M
   near call $$CONTRACT_FAUCET_ID new '{"owner_id": "'$MAIN_ACCOUNT'", "ft_contract_id": "'$CONTRACT_FT_ID'", "max_share": 10000000}' --accountId           $MAIN_ACCOUNT

   #### 3. Update contract: with max_share: 30M
   near call $CONTRACT_FAUCET_ID update_max_share '{"max_share": "30000000"}' --accountId $MAIN_ACCOUNT

   #### 4. Update contract: Total token in contract 1B, total_share 10M
   near call $CONTRACT_FAUCET_ID update_pool '{"total_balance_share": "1000000000", "total_share": "10000000", "total_account_share": "1"}' --accountId      $MAIN_ACCOUNT

   #### 5. Account faucet token
   account (faucet) faucet 1M Token 
   near call $CONTRACT_FAUCET_ID faucet_token '{"amount": "1000000"}' --accountId $CONTRACT_FAUCET_ID --deposit $ONE_YOCTO --gas $GAS

   account (test1) faucet 2M Token 
   near call $CONTRACT_FAUCET_ID faucet_token '{"amount": "2000000"}' --accountId $ACCOUNT_TEST1 --deposit $ONE_YOCTO --gas $GAS

   account (test2) faucet 3M Token 
   near call $CONTRACT_FAUCET_ID faucet_token '{"amount": "3000000"}' --accountId $ACCOUNT_TEST2 --deposit $ONE_YOCTO --gas $GAS

   ##### 6. Get info faucet
   near call $CONTRACT_FAUCET_ID get_faucet_info '' --accountId $CONTRACT_FAUCET_ID

   ##### 7. Get info balance
   near call $CONTRACT_FAUCET_ID get_share_balance_of '{"account_id": "'$ACCOUNT_TEST1'"}' --accountId $MAIN_ACCOUNT
   near call $CONTRACT_FAUCET_ID get_share_balance_of '{"account_id": "'$ACCOUNT_TEST2'"}' --accountId $MAIN_ACCOUNT
  
</sup>
