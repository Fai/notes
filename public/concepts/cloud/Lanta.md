[Training Material](https://thaisc.atlassian.net/wiki/spaces/LANTA/pages/700743723/All+training+materials)

Lanta เป็นเครื่อง High Performance Computer ในโครงการ Thai Super Computer

วิธีการ ตั้ง ssh key เพื่อเข้าใช้ Lanta โดยไม่ต้องใส่รหัสผ่าน

```
ssh-keygen -ed25519
vim .ssh/authorized_keys
ssh ai5002@lanta.nstda.or.th -i ~/.ssh/ruchida
```

HPC Command

```echo "alias myqueue='squeue -u $USER'" >> ~/.alias
source ~/.alias```
