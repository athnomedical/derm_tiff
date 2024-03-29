# 使い方

## [DermAnnotation](https://kondoa9.github.io/DermAnnotation/en/)で作成されたTIFF画像を開き，各フレームのラベルを取得
```py
from derm_tiff import load_image

derm_image = load_image("example/imgs/input.tiff")
labels = derm_image.labels
print(labels)  # ['Pencil1', 'Pencil2', 'Pencil3']
```

## `DermTiffImage`の新規作成
```py
assert isinstance(bg_image, NDArray[Shape["*, *, 3"], np.uint8])
H, W, C = bg_image.shape

empty_derm_image = DermTiffImage(bg_image=bg_image)
new_derm_image = DermTiffImage(bg_image=bg_image,
                               label2mask={"page1": np.zeros((H, W),
                                                              np.bool_)},
                               label2color={"page1": (255, 0, 255)})
```

## TIFF画像の各フレームのマスクを合成して出力
```py
for label in labels:
    pil_img = derm_image.get_annotation_image([label], alpha=0.5)
    pil_img.show()
```
![Pencil1](imgs/with_Pencil1.png)![Pencil2](imgs/with_Pencil2.png)![Pencil3](imgs/with_Pencil3.png)


## 新たにフレームを追加
```py
import numpy as np

H, W, _ = derm_image.shape
new_label = "new label"
new_mask = np.random.randint(0, 2, (H, W), dtype=np.bool_)
new_color = (255, 255, 0)
derm_image.add_frame(new_label, new_mask, new_color)
```

## DermAnnotationで開けるTIFF形式で保存
```py
derm_image.save("example/imgs/output.tiff")
```

## 全体でリサイズも可能
```py
resized = derm_image.resize(300, 200)
```