# Work log

Here goes somewhat rough detailing of the approaches and recording the experiments.

## Approaches

Started with xgb model (ik, should have started with something simpler). params as follows:

```py
xgb_params = {
        "subsample": 0.7,
        "colsample_bytree": 0.45,
        "max_depth": 6,
        "learning_rate": 0.006,
        "objective": "reg:squarederror",
        "nthread": -1,
        "max_bin": 168, 
        'min_child_weight': 3,
        'reg_lambda': 0.006,
        'reg_alpha': 0.04, 
        'seed' : 42,
}
```

did not transform the target to `log1p` and trained with mse. For cross-validation, transformed the target to `log1p` and quantile cut into 7 labels, followed by stratified split.

> Apparently, I applied quantile cut on the non-transformed target for stratified split. Lemme try with transformed target once.

## Experiments

| model | params | loss | target-transform | cv-strategy | cv-local | n-fold-cv | lb |
|-------|--------|------|------------------|-------------|----------|-----------|----|
| xgb | default | se | no | stratified split on qcut non-transformed target | 0.027352314045830044 | [0.027179122642949263, 0.027190944459390628, 0.02727586429230058, 0.027577436182662254, 0.027535531339567026] | 1.72035 |
| xgb | default | se | no | stratified split on qcut **transformed target** | 0.017514957855121846 | [0.017425094761927922, 0.017483438838756755, 0.01726607380938987, 0.017773971074770266, 0.017621941536573513] | 1.77202 |
| xgb | default | se | yes | stratified split on qcut **transformed target** | 0.014021348769779956 | [0.013745017545829788, 0.014177045107707686, 0.013876652354521834, 0.014031799715188161, 0.01426969204663097] | 1.61971 |
