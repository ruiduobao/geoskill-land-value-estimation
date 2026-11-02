# 土地价值估算 | Land Value Estimation

用特征价格（Hedonic）模型估算土地价值空间分布，服务于地价评估与区位分析。

核心算法：可达性 = exp(−到中心距离/衰减尺度)，越靠近中心越高；绿地邻近性同理；价值 = 截距 + β_acc×可达性 + β_poi×POI密度 + β_green×绿地邻近性。模型线性可加、价值非负；附带最小二乘系数标定函数，可由样本精确恢复已知系数。

## 依赖

```bash
pip install 'numpy' 'rasterio'
```

## 使用方法

### 基本用法

```bash
python geoskill-land-value-estimation.py --bbox 116.0 39.0 117.0 40.0 [其他参数]
```

### 示例

#### 示例 1（合成数据（离线））

```bash
python geoskill-land-value-estimation.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

#### 示例 2（用法 2）

```bash
python geoskill-land-value-estimation.py --input features.tif --output-dir ./out
```

#### 示例 3（用法 3）

```bash
python geoskill-land-value-estimation.py --bbox 121.0 31.0 122.0 32.0 --decay 50 --output-dir ./out --quiet
```

#### 示例 4（用法 4）

```bash
python geoskill-land-value-estimation.py --input features.tif --coef-acc 8000 --intercept 2000 --output-dir ./out
```

#### 示例 5（用法 5）

```bash
python geoskill-land-value-estimation.py --bbox 116.0 39.0 117.0 40.0 --synthetic --coef-poi 3000 --output-dir ./out --quiet
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `land_value.tif` | GeoTIFF | 土地价值栅格 |
| `hedonic_coefficients.json` | JSON | Hedonic 模型系数与衰减尺度 |
| `value_stats.json` | JSON | 价值均值/范围、平均可达性 |
| `output-manifest.json` | JSON | 运行清单 |

## 数据源 / Source

本地三波段特征 GeoTIFF（到中心距离、POI 密度、到绿地距离）；`--synthetic` 模式模拟单中心城市格局。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
