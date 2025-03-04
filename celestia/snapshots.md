# 🚀 Restore Celestia Node from [Posthuman](https://posthuman.digital/) Snapshots

This guide explains how to restore your Celestia node using a snapshot from **Posthuman**.

---

## **🛑 Step 1: Stop Celestia Node**
Before restoring, stop the Celestia process to prevent database corruption:

```bash
sudo systemctl stop celestia-appd
```

---

## **📌 Step 2: Backup Validator State (IMPORTANT)**
To avoid double signing issues, **backup your validator state file**:

```bash
cp $HOME/.celestia-app/data/priv_validator_state.json $HOME/.celestia-app/priv_validator_state.json.backup
```

---

## **🗑 Step 3: Remove Old Blockchain Data**
Delete the old data to **free space** and **prevent conflicts**:

```bash
rm -rf $HOME/.celestia-app/data
```

---

## **📥 Step 4: Download & Extract the Latest Posthuman Snapshot**
> **Note:** Since Posthuman updates snapshots **every 24 hours**, use the latest one:

```bash
curl -L https://snapshots.celestia.posthuman.digital/data_latest.lz4 | lz4 -dc - | tar -xf - -C $HOME/.celestia-app
```



---

## **📂 Step 5: Restore Validator State**
Move back the **backup validator state file**:

```bash
mv $HOME/.celestia-app/priv_validator_state.json.backup $HOME/.celestia-app/data/priv_validator_state.json
```

---

## **▶️ Step 6: Restart Celestia Node & Monitor Logs**
Now, restart the service and monitor its logs:

```bash
sudo systemctl restart celestia-appd
sudo journalctl -u celestia-appd -fo cat
```

---

## **✅ Done!**
Your node should now sync from the restored **Posthuman snapshot**. 🚀 

