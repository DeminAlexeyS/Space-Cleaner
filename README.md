# Space Junk Runner — пакет для разработки

Игра на основе исходного `index.html` (манифест ассетов, вся графика подключается по ключам). Арт заменён на нарисованный, интерфейс переделан под него.

## Что внутри
```
index.html       игра (один файл, код + стили)
art/             все картинки, названы по ключам: ship.scout -> art/ship_scout.png
art_unused/      то, что нарисовано, но пока не подключено (запасной враг, свалка-астероиды)
fonts/           Fredoka (latin, 400 и 700), лицензия OFL внутри
```

## Как запустить
Нужен обычный статический сервер, из папки с `index.html`:
```
python3 -m http.server 8000
```
Открыть `http://localhost:8000`. Если открыть файл двойным щелчком, часть браузеров не даст загрузить шрифт и картинки.

**Гироскоп.** Браузеры отдают датчики только на https (на iPhone ещё и запрашивают разрешение по кнопке «Enable tilt»). Чтобы проверить на телефоне, выложи папку на любой https-хостинг (Netlify Drop, GitHub Pages) или используй `adb reverse` для локального сервера. Без датчика работает управление пальцем.

## Что изменено в коде относительно исходного index.html
1. **ASSETS:** процедурные записи заменены на `type: 'image'`: 6 кораблей, враг, junk, junk.rare, 3 бонуса, астероиды (несколько вариантов на размер и наборы по секторам), ангар. Добавлены ключи `asteroid.<набор>.<размер>`, `debris.1-8`, `planet.*`.
2. **Assets.draw:** для картинок добавлены подкраска под цвет сектора (флаг `sectorTint`, только у каменных астероидов, у остальных наборов цвет свой) и белая вспышка врага при попадании (`_scratch`).
3. **Assets.drawScene:** ангар масштабируется по большей стороне (cover), чтобы заполнять высокие экраны.
4. **Астероиды по секторам:** `SECTOR_ROCKS` + `pickRock()` (лёд, лава, аметисты, кристаллы, каменные; ржавый сектор пока тоже каменный). Варианты внутри размера выбираются случайно (`CONFIG.asteroid.keys` может содержать массивы).
5. **Фон:** блок `DECO` (планеты по секторам, мусор в среднем слое, кометы; три слоя глубины с разным размытием и затемнением, настройка в `DECO.LAYERS`). Звёзды: блёстки и редкие цветные гиганты (`sparkle`, `giants` в `makeStarLayer`). Цветная подложка секторов сделана прозрачнее.
6. **Меню и ангар:** затемняющая вуаль поверх ангара (`CONFIG.menu.hangarDim`), крупный корабль (`shipScale`), логотип-картинка, «Tap to start» в две строки. Магазин переписан: корабль (в апгрейдах иконка бонуса) сверху, внизу горизонтальная карусель карточек (`UI.renderHangar`, `previewSkin`, `shopArea`). Корабль вписывается в реальную свободную полосу между счётчиком и карточками.
7. **Интерфейс:** плиты и кнопки рисуются объектом `Plates` как честный 9-slice на canvas под точный размер (никаких швов и сплющенных заклёпок на дробных плотностях пикселей). Картинки плит заданы в `PLATE_ART`, размеры углов в `Plates.SPEC`. Иконки (`ICONS`), сердца и пауза в HUD картинками. Шрифт Fredoka.
8. **Управление:** палец теперь работает и при активном гироскопе (смещение поверх наклона и плавный возврат, `Input.tiltBias`). Раньше при активном датчике касание игнорировалось.
9. **Тексты:** функция `noWidow` (число не отрывается от единицы, стрелка не стоит в начале строки). Строка «Tilt to steer…» убрана с главной.

## Ключи и файлы
| Ключ | Файл | Размер |
|---|---|---|
| `ship.scout` | `art/ship_scout.png` | 512×678 |
| `ship.ember` | `art/ship_ember.png` | 512×659 |
| `ship.viridian` | `art/ship_viridian.png` | 512×694 |
| `ship.phantom` | `art/ship_phantom.png` | 512×660 |
| `ship.aurum` | `art/ship_aurum.png` | 512×671 |
| `ship.nova` | `art/ship_nova.png` | 512×642 |
| `enemy` | `art/enemy.png` | 512×824 |
| `junk` | `art/junk.png` | 256×256 |
| `junk.rare` | `art/junk_rare.png` | 256×256 |
| `pickup.shield` | `art/pickup_shield.png` | 256×256 |
| `pickup.magnet` | `art/pickup_magnet.png` | 256×256 |
| `pickup.rocket` | `art/pickup_rocket.png` | 256×256 |
| `hangar` | `art/hangar.jpg` | 1440×2580 |
| `asteroid.huge` | `art/asteroid_huge.png` | 1024×1024 |
| `asteroid.huge2` | `art/asteroid_huge2.png` | 1024×1024 |
| `asteroid.large` | `art/asteroid_large.png` | 512×512 |
| `asteroid.large2` | `art/asteroid_large2.png` | 512×512 |
| `asteroid.large3` | `art/asteroid_large3.png` | 512×512 |
| `asteroid.large4` | `art/asteroid_large4.png` | 512×512 |
| `asteroid.medium` | `art/asteroid_medium.png` | 384×384 |
| `asteroid.medium2` | `art/asteroid_medium2.png` | 384×384 |
| `asteroid.medium3` | `art/asteroid_medium3.png` | 384×384 |
| `asteroid.medium4` | `art/asteroid_medium4.png` | 384×384 |
| `asteroid.medium5` | `art/asteroid_medium5.png` | 384×384 |
| `asteroid.small` | `art/asteroid_small.png` | 256×256 |
| `asteroid.small2` | `art/asteroid_small2.png` | 256×256 |
| `asteroid.small3` | `art/asteroid_small3.png` | 256×256 |
| `asteroid.small4` | `art/asteroid_small4.png` | 256×256 |
| `asteroid.small5` | `art/asteroid_small5.png` | 256×256 |
| `asteroid.crystal.huge` | `art/asteroid_crystal_huge.png` | 481×481 |
| `asteroid.crystal.large` | `art/asteroid_crystal_large.png` | 382×382 |
| `asteroid.crystal.medium` | `art/asteroid_crystal_medium.png` | 374×374 |
| `asteroid.crystal.small` | `art/asteroid_crystal_small.png` | 256×256 |
| `asteroid.ice.huge` | `art/asteroid_ice_huge.png` | 478×478 |
| `asteroid.ice.large` | `art/asteroid_ice_large.png` | 427×427 |
| `asteroid.ice.medium` | `art/asteroid_ice_medium.png` | 384×384 |
| `asteroid.ice.small` | `art/asteroid_ice_small.png` | 256×256 |
| `asteroid.lava.huge` | `art/asteroid_lava_huge.png` | 478×478 |
| `asteroid.lava.large` | `art/asteroid_lava_large.png` | 390×390 |
| `asteroid.lava.medium` | `art/asteroid_lava_medium.png` | 381×381 |
| `asteroid.lava.small` | `art/asteroid_lava_small.png` | 256×256 |
| `asteroid.void.huge` | `art/asteroid_void_huge.png` | 535×535 |
| `asteroid.void.large` | `art/asteroid_void_large.png` | 418×418 |
| `asteroid.void.medium` | `art/asteroid_void_medium.png` | 384×384 |
| `asteroid.void.small` | `art/asteroid_void_small.png` | 239×239 |
| `debris.1` | `art/debris_1.png` | 244×256 |
| `debris.2` | `art/debris_2.png` | 256×211 |
| `debris.3` | `art/debris_3.png` | 256×236 |
| `debris.4` | `art/debris_4.png` | 256×250 |
| `debris.5` | `art/debris_5.png` | 255×167 |
| `debris.6` | `art/debris_6.png` | 145×256 |
| `debris.7` | `art/debris_7.png` | 256×212 |
| `debris.8` | `art/debris_8.png` | 256×165 |
| `planet.desert` | `art/planet_desert.png` | 338×337 |
| `planet.ice` | `art/planet_ice.png` | 335×336 |
| `planet.lava` | `art/planet_lava.png` | 336×336 |
| `planet.moon` | `art/planet_moon.png` | 334×335 |
| `planet.ocean` | `art/planet_ocean.png` | 336×338 |
| `planet.ringed` | `art/planet_ringed.png` | 541×332 |
| `planet.stripe` | `art/planet_stripe.png` | 343×344 |
| `planet.violet` | `art/planet_violet.png` | 453×338 |

Иконки и интерфейс лежат там же: `icon_*.png`, `ui_*.png`.

## Что ещё не заменено (рисует код, как в исходнике)
`bullet`, `rocket` (снаряд), `background.far/mid/near` (звёзды), `shield.aura`, `magnet.field`, подсветка посадочной площадки. Враг без отдельного кадра «hit» (вспышка делается кодом).

## Где подкрутить
- `CONFIG.menu`: `shipScale`, `hangarShipScale`, `hangarDim`.
- `DECO.LAYERS`: размер, скорость, размытие, затемнение планет и мусора.
- `SECTOR_ROCKS`: какой набор астероидов к какому сектору.
- `Plates.SPEC`: размеры углов плит и кнопок.

## Известные ограничения
- Многие листы были сгенерированы в 1K, поэтому у огромных астероидов, планет и плит запас по резкости небольшой. Для финала лучше перегенерировать в 2K.
- Три иконки апгрейдов (сердце, патрон, шестерёнка) резкие до ~330 px, больше исходник не даёт.

## Лицензии
Шрифт Fredoka: SIL Open Font License 1.1 (файл в `fonts/`). Картинки сгенерированы в Google Flow и обработаны (нарезка, вырезка фона, подгонка размеров).
