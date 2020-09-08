# Experiment in Business Process Modelling in DAML

_work in progress_

```
commercialFinancing = scenario do

  sales <- getParty "sales"
  risk <- getParty "risk"
  admin <- getParty "administration"
  credit <- getParty "credit"

  cid <- issueProcess do

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