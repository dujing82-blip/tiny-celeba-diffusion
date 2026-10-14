# Prompt vocabulary: 40 original attributes

Each row is an annotated concept, not a general-language word. Use the positive phrase or put `not` before it for the opposite state. `no` and `without` also negate the following concept. Unspecified attributes remain unknown.

There are 80 positive/negative state tokens and one unspecified token. Aliases share tokens and do not add new annotations.

## Common prompts

`smiling male` · `a female with glasses` · `smiling female with blond hair and no glasses` · `a male with a beard` · `female without glasses`

## All original attributes

| Original annotation | Positive prompt spelling | Opposite spelling |
|---|---|---|
| `5_o_Clock_Shadow` | `5 o clock shadow` | `not 5 o clock shadow` |
| `Arched_Eyebrows` | `arched eyebrows` | `not arched eyebrows` |
| `Attractive` | `attractive` | `not attractive` |
| `Bags_Under_Eyes` | `bags under eyes` | `not bags under eyes` |
| `Bald` | `bald` | `not bald` |
| `Bangs` | `bangs` | `not bangs` |
| `Big_Lips` | `big lips` | `not big lips` |
| `Big_Nose` | `big nose` | `not big nose` |
| `Black_Hair` | `black hair` | `not black hair` |
| `Blond_Hair` | `blond hair` | `not blond hair` |
| `Blurry` | `blurry` | `not blurry` |
| `Brown_Hair` | `brown hair` | `not brown hair` |
| `Bushy_Eyebrows` | `bushy eyebrows` | `not bushy eyebrows` |
| `Chubby` | `chubby` | `not chubby` |
| `Double_Chin` | `double chin` | `not double chin` |
| `Eyeglasses` | `eyeglasses` | `not eyeglasses` |
| `Goatee` | `goatee` | `not goatee` |
| `Gray_Hair` | `gray hair` | `not gray hair` |
| `Heavy_Makeup` | `heavy makeup` | `not heavy makeup` |
| `High_Cheekbones` | `high cheekbones` | `not high cheekbones` |
| `Male` | `male` | `not male` |
| `Mouth_Slightly_Open` | `mouth slightly open` | `not mouth slightly open` |
| `Mustache` | `mustache` | `not mustache` |
| `Narrow_Eyes` | `narrow eyes` | `not narrow eyes` |
| `No_Beard` | `no beard` | `not no beard` |
| `Oval_Face` | `oval face` | `not oval face` |
| `Pale_Skin` | `pale skin` | `not pale skin` |
| `Pointy_Nose` | `pointy nose` | `not pointy nose` |
| `Receding_Hairline` | `receding hairline` | `not receding hairline` |
| `Rosy_Cheeks` | `rosy cheeks` | `not rosy cheeks` |
| `Sideburns` | `sideburns` | `not sideburns` |
| `Smiling` | `smiling` | `not smiling` |
| `Straight_Hair` | `straight hair` | `not straight hair` |
| `Wavy_Hair` | `wavy hair` | `not wavy hair` |
| `Wearing_Earrings` | `wearing earrings` | `not wearing earrings` |
| `Wearing_Hat` | `wearing hat` | `not wearing hat` |
| `Wearing_Lipstick` | `wearing lipstick` | `not wearing lipstick` |
| `Wearing_Necklace` | `wearing necklace` | `not wearing necklace` |
| `Wearing_Necktie` | `wearing necktie` | `not wearing necktie` |
| `Young` | `young` | `not young` |

## Helpful aliases

| Prompt wording | Original annotation condition |
|---|---|
| `female`, `woman` | Male=False |
| `male`, `man` | Male=True |
| `glasses`, `eyeglasses` | Eyeglasses=True |
| `smile`, `smiling` | Smiling=True |
| `beard` | No_Beard=False |
| `no beard`, `clean shaven` | No_Beard=True |
| `blonde hair`, `blond hair` | Blond_Hair=True |
| `grey hair`, `gray hair` | Gray_Hair=True |
| `hat`, `lipstick`, `earrings`, `necklace`, `necktie` | Corresponding Wearing_* annotation=True |

The parser ignores `a`, `an`, `the`, `with`, `and`, `face`, and `person`. It accepts spaces, underscores and hyphens as separators. It rejects unsupported concepts instead of silently pretending to understand them.

`male female`, `glasses no glasses`, and other contradictory conditions are errors. The printed condition dictionary is the authoritative interpretation of a prompt.

These are the source dataset's appearance annotations, not verified truths about a person's identity. Some source annotations are subjective. Supported tokens do not guarantee that a trained model will faithfully render every attribute, especially rare ones.

Sources: [official CelebA](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html), [attribute mirror and schema](https://huggingface.co/datasets/flwrlabs/celeba/tree/2d738f56e0e7f925ea36ae7c808ea925264aacec).
