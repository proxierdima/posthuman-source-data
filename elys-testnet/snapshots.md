# 🚀 Restore Elys Node from [Posthuman](https://snapshots.kyve.posthuman.digital/) Snapshots

This guide explains how to restore your Kyve node using a snapshot from **Posthuman**.

---

## **🛑 Step 1: Stop Elys Node**
Before restoring, stop the Kyve process to prevent database corruption:

```bash
sudo systemctl stop elys
```

---

## **📌 Step 2: Backup Validator State (IMPORTANT)**
To avoid double signing issues, **backup your validator state file**:

```bash
cp $HOME/.elys/data/priv_validator_state.json $HOME/.elys/priv_validator_state.json.backup
```

---

## **🗑 Step 3: Remove Old Blockchain Data**
Delete the old data to **free space** and **prevent conflicts**:

```bash
rm -rf $HOME/.elys/data
```

---

## **📥 Step 4: Download & Extract the Latest Posthuman Snapshot**
> **Note:** Since Posthuman updates snapshots **every 24 hours**, use the latest one:

```bash
curl -L https://snapshots.elys-testnet.posthuman.digital/data_latest.lz4 | lz4 -dc - | tar -xf - -C $HOME/.elys
```



---

## **📂 Step 5: Restore Validator State**
Move back the **backup validator state file**:

```bash
mv $HOME/.elys/priv_validator_state.json.backup $HOME/.elys/data/priv_validator_state.json
```

---

## **▶️ Step 6: Restart Kyve Node & Monitor Logs**
Now, restart the service and monitor its logs:

```bash
sudo systemctl restart elys
sudo journalctl -u elys -fo cat
```

---

## **✅ Done!**
Your node should now sync from the restored **Posthuman snapshot**. 🚀 

