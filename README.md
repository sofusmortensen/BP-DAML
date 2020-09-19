# Experiment in Business Process Modelling in DAML

_work in progress_

```
commercialFinancing = script do

  sales <- allocateParty "sales"
  risk <- allocateParty "risk"
  admin <- allocateParty "administration"
  credit <- allocateParty "credit"

  cid <- issueProcess sales do

    ack sales "Consolidate and Digitize Data"
    ack sales "Analyze Financial Data"
    select sales "Qualified Request?" do
      ch "Yes" do
        ack credit "Verify Credit Record with Credit Offices"
        ack risk "Sign-off from Risk"
        ack admin "Prepare Detailed Analysis of Financial Data"
        ack sales "Prepare Detailed Analysis"
        select sales "Qualified Request?" do
          ch "Yes" do
            ack sales "Elaborate Commercial Financing Reporting"
          ch "No" do
            ack sales "Prepare and Transmit Refusal Letter"
            cancel
      ch "No" do
        ack sales "Prepare and Transmit Refusal Letter"
        cancel
```

The business process above will initially have only `sales` as signatory, and only `sales` as observer. As the flow proceeds parties required to act will be become observers, and when they have acted they will become signatory. 

Each `ack` step will generate an instance of `BusinessProcess_Service` with the party required act, the message and with all parties signed so far as signatory parties.