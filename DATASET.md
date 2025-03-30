# Dataset Preparation
## ZJU-MoCap
Please reach out to the TAs to get the dataset.  After you get the dataset, extract the dataset to an arbitrary directory, denoted as ${ZJU_ROOT}. It should have the following structure: 
```
${ZJU_ROOT}
 ├-- CoreView_313
 ├-- CoreView_315
 |   ...
 └-- CoreView_394
```

After this, create a symbolic link under `./data` directory by:
```
ln -s ${ZJU_ROOT} data/ZJUMoCap
```
