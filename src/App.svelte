<script lang="ts">
  let input = "";
  let input_field: HTMLInputElement;

  $: output = input.replace(/\D/g, "");

  $: links = [
    {
      link: `https://www.nhentai.net`,
      href: `https://www.nhentai.net/g/${output}`,
      // https://static.nhentai.net/img/logo.090da3be7b51.svg
      logo_url:
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAeMAAADRCAYAAAD/stoqAAAgAElEQVR4Xu19CXxdVZ3/OfclTVsoNM0CMkC3LHUAUZB9U5BNoTRJgwj8WXTcmEFFUHFUFlEU1AFHFEZFQESZaZO0BUZkEURAFkFUUPKSLpQO0iQvTfc0ee+c//e8l5S2tMl97+73fm8/ty95Oef3+53v79zzvWf7HSlCemkh5JpZrfuJ8tx+Oa33kyI1Syixu0iJ/bVQe0ht7aaF3g3mT5JCTMbnRC3lRKl1hZaiQmixSlpIq3VOajkspRxWWg1LIYchewh5tuDnQSH1JqTphjolpOzRWvXgbz1SWatzMtsjh6zV1a+1r8Z3KqRQ0SwiQASIQGgR0KI11Tc9V6snqL1SuqxWW2ovtMG1Ulq1QutaIbSF9rlOaDkZbfpE/K0C7e0E/FxuSasc7XO5lrocaVJaiZVCin2lFlvQ3m9Bez+Igg8izyZ8bkabvtGSej0a8w1Cq5X4bj3kLE9J+boYTr1euWzB65CN5OG7YFdwV8/suXVlVmpOTop6S1j7Kqn/GaQ4E0btDSftAXI0IEv8PhEf+AdY8X8eS/OtgRRegRPNn0e+Lvxuvhq9RvONJBlJCkkmXT5RXpD5zBrH4tcs5EMZEkhRjt8n4adNSJHBd2nk7JNS/RlK0lLJ9LTuFWkpXhgODklqJgJEgAgEg4AWh5b3181o0FI1Cmk1oJ18FyypQZvZgJZ1GtrZyWhsN6MdHR5pVy20q2Voc01napv2t9CeF1r6rU36Nu1+IbVp8s1/pnkebfe34wGTO08Ab3Eufho0WfCd4ZK1+PwHyH0Fvvm7EmpVSqe6smro1dqlS9AxC+byhYwNdfbWtbxbWvpQKDwODjkE39UDnM0AZQB/noo3nz3hobwrRtAOBpGxtOrRl4C8k3O4YX/+DWEy3g0G8OeVsP5VfPUCaspfrKxMVy5vfy18BaFFRIAIEIHiEFgzs3m6SlmNIN13o717DxrqOeC8/dH+7Ylm27SF5jKjlIZsR+i0OB2+pTZNeb7pFgrkvzan1VoYPRX6J+IP3fjDC2Cj3+dy8sWa7raX/OhNe0LGffVnHaGssiNTQhyDwh6GAk43wwhgWlNQfD3yRuMb8j4pynP01r42hsHzhG3hsxNE/aRU+nGrTP2x8tVFK3yyiGqIABEgAkUjsGbOvBnDWh6GEcsTkfkYNGt1Ix1W9GjFhILA2Lfj2XyPWghMiYoVltbP5qR8xlLZZ6q7Fj9bNKjjZHCFjAfq5s3OplKnQFfriONW441pmilEbB1WjCdGSBofowRtvvk7KvijGBF4WojsH2s6l7xRjEimJQJEgAi4gUBv49x90Ec6TArrWDRMhnznoPOAUVzTcTLEuz3pbj8A7IYFEZDxVkdrPaxdg3sv3E/ibivL5R6a2r1oqdNSlETGPTWtu1uVuZOR+WxMop8EIybBYeaNyfR8edlBoOBcjJPI9SDkFGDDYgTxCF5iHsfUym9Bzp12xDANESACRKAYBEC+jVKXnYT53fejCfqAWRczMsu6OztPRSBZaMPNwrEcOlabMb36MD7b1JrUw7W9CzYUISmf1DZ19s9uOlClUnOh/Dy8MdXhxQmrizEZbhZS8XIHga3OlVhIpjHErR8FWd+P4e1Hq7rbV7mjhFKIABFIEgKZ2Wftp1NlJ+JFvwkv/CeMTBWWs/Pkci0wfSszHSvM4jSRRtv9KyuXWzJtacfLdjSNyaRr922dNjQ5eyGWoH8awqryzhNmZZwd0UzjGAGzYEzIdWbpf/5T6QeVpR6s2Fj26J6rFvQ7lk8BRIAIxA4B025v2S17kqWtD6GtNtOHU0Y6Xrux8+STu03TrfU6jHpiG62VwXbc2yZsTP18rHZ7p7TaW998rrTk5RB3kFnxjC7wptFpA3B/GYRPMHu/0CnG8LS5YjqR75PfbKvJk7MwJDwRP/4R90MpLRZVdbe9YlsGExIBIhA7BMzIZbZMzkN7cCr22JpdK1vQLE8l+frg6m0X7oIvTUwLkC9IWJoFYPk1vehNIQ4GVppr/WdsybqppqvtVztatpWMVzc2nZfS1mXIcEh+C5fQPRD6lNlLi672X7KWmIRlwTnMMyB2hsCiMjUBymZguHomnF4PQVOQ6QDk60U+MwQyjRXBw4pQ8LJZTGD2N2Ozu/glluovntbZ/pSHWimaCBCBkCDQ0zD/OEuoVrSzWDgrK9D2IgCS5LodL/1T6BAh3gSmEmU+YMkr4MC1lhZdSugVCDG1AsGmhnCnEHQEZClS6L2iQ2sdjHzvRO4jsIWqNj+gLSQ6VNmba7oW5YlZrq+b/74tKf0b/JxFb/dxbND+enVXW0nLts3WrUx9y+Ew6vCUJY+FIQ3oPc+CbDPMujuHtz2sJVj1BTJeBx8gSIlux9vxgtp02/96qJGiiQAR8BmB3sbmuZaW56DThPU7Ygh3JTs9HjrhrU6P4c+l+BUBnswq6txz1elFz5Wqub+x+Zictr6CvcwnQG4ZXqpOkv14u1JiuKI6vfiRUgWPlW+gsXXmkMqeiXhn56HSvBsvFmvRc64hMXuB9ohMM66hEdIT28vg6CV4Q7unuquj3UONFE0EiIBHCPQ0tpyd0voC9MBOxxxkP3q/1SRgj8A2YvPzvWo1eKoS7eiLSutflmfFEq8COGXq5p2C2d8Nvi7F0vucOTkzueJkbenPgygOyRdbSrO4wJML3XEGlNbaDGP/A/fegPsepa07atMLf+8J4BRKBIiAKwigB/x+kO6/oIX8MDoub2Jk8R34NE0aL68QUIgGmQ/TKZ4XOfX9qsGyh+SqBaORxbzSulWur2S8bWl6ZrTuLSbkPop56H9FRQNhyOnsLXvs7/zSe8TVxmkYltA/zWblnbXL2ro81krxRIAI2ECgr6FlDjplH0dH5aKR5Fx3YwM3R0nM3KrWyzHsjwAn6kcVWXHHlKUdPY5klpg5MDLe1t5MXdNROmVdje8QOlNWgJRHAoiXWCpmGx8BrTHfhFOrEDAdW6Z+qK3snQg0YhaE8SICRMAnBPpnte6ZS2VBwNan0O5hQRBW3OJ0Ip/UJ1eN1jgsIh+s4+nyYeu6PZcvKHn+1y0QQ0HGo4XpndXUIMvllei9fdjE8Mb2KpKyW57elRzzZij0UH5tnxCPK6X/s6ar/QGv1VI+EUgyAr31TWeAdK/EvOThwAG7T7g91I/6gJ1CazAHPBHHLP48lVPfcSOMpVt2h4qMRws1sP+HKtXEiZ/EcRpXg5S34KiFPd0qMOWMgUB+5aAwQUaG0Vu+uWrdhpvk6oc2EjMiQAScI4AwlFMsVfYFhBC+FI9aBYh4EqfmnONqRwJm6PrQrk0RSn3V0oM/rur+Ndq5cF2hJONRiEZiYN+AN8jzRypvRbjgi6A1dqO854ex8+e0PID726Vud4sgQjSZCLiKQKau+UgxQV6DALfmEAZzUiyHoV1FeNfCsF13A3rD2Okp7qxaP/Rl+cZ9JpZ0KK9Qk/EoYr3Tm99hVYj/AKBHamHN4Nukj3WpsMl9HQi5FyMV19d2tv3MR+1URQQii0BPY/PHwLxX4RGqyUcrZBx//3xpTr4QugtE/MzE4eEv7L58yWr/lJemKRJkvJWU83PK1j34fRYq9rTSisxcJSEwMoSNrvJEzOb/wBrK3lK5gucyl4QlM8UWgYH9W2ZlK/QVmFr7ONhgGO0Uh6JteNvVbahavwliW5balLtg6uvOjza0Yb4rSSJFxltJ2cTOlvKn+T1hhdCbvPxEAEM/aGhWQmVaK31dTXf7i36qpy4iEDYEehtb3ouQiNci+mAj2qb90C7hHGBe/iKgN2EUAoEfxccwgvc//up2ri2SZDxa7Exj821YeX0+pjZxGolzMCihSAQKAUUGMBT0qpbqmurOjt8WKYHJiUCkEcDe4A9gQPSbIN96tEMITel/cewuA/HfMp80Fk5IWov54TuwE+Qyn7S6riaAquNuGcxGeTwAv4bUKi+jeblrdcykmbdRrcwBIb045fqaqq62tpiVkMUhAtsh0NPY1GoJ65v4sjooEqZLDAmLjSDhf5Sr3Glh2qZUim8iT8Zbe8n1zV/DloHLQcw4kIKrFUupDI7z5N9QBRZK6GEl9TW1ne23O5ZJAUQgRAjgeNlPIkDHtajjOB0JWy5j04KGCGS7pmi9Ed0A7PRo/4bdLGFOF6uqtGbOvBk5lfoBHpDj8g8Kr+AQ0DoD5eUip79c3d3+o+AMoWYi4BwBRAm8DFECr8u/bkqcQMcrOAS03oyDHJ6WOXVx1dLFrwdniLuaY0XGo9D0NTRfj6GjSxgsxN3KUpI0kDI6zJOk0lfiDfYHJclgJiIQEAKZhubLlZTfxC4Cc5b75IDMoNpRBMxhDkLcVp1u+3LcQIklGRsnmY32ypKPYR6Th22HoNYiGHu/MSOn1BW1XR13hMAkmkAEdolAf33Lp9B+3ICOsNmxQRIOuK5gXngLTMhKnTvRyTnCARdjTPWxJeOtveT65oXakqeBlBnnOgw1MU/KclCJ3OdqOzsWhMEk2kAERhHob2w5H6cH3IQ6OhltBkk4DFXDtBla3Ife8EVhMMcrG2JPxgY4rLi+Em+3X8RccqVXQFJukQhguAnHlvVjX+Cltem2/y0yN5MTAVcRMAc3SMu6DULNsYWTXBXuqzBXw2f4avlOlSGAB4j4myDiW4I3xlsLEkHGBsLM7PlHizL9O6y4NodH8zIIBP3c5qN66V7897oayn6udvni39MxRMBPBHrqm4+3LPlj6Pwn9IaxE8NP7dQ1JgJKI4aKODopcfETVfW0OLQ80zD9Cbz5HsbtTy42BE5JHRNC4OXliGDUKXLZT8ZphaSLKFOUiwismdk8PTdB3oFwTQdaUtaQhF0E16kordeDhJ+tSredAoIqBOJNwJUoMh71Z6ah5RH0kI/BAzgxAT6OTBHNIg1E89qMp++OmnTb5yNjOA2NFAJ9jS23wmBE7hOI3MfTG0LlPLMlUusnqtPtzaGyywdjEknGBlfEkr0VhT+PUbt8qGXjqtghoJ/W5qzRiRijuoSBQ8YFjwlsItDX0PSv2J50M5KbFdI75Ep8UEmbKHqYrDA/fGccty3ZQS2xZGzAwcKuf0Mv7CppYZiKV/gQKGyHyigtP1abXsj55PB5KBIW9dY1nWClrLvwrJuQuQzYEUKv4cCZ5VIgvn264+chNM8XkxJNxgbhnllNx1rlqSVcae1LfSteSf5cUtEnlXphYjZ7YRTOJS2+kMzhBQLmHHRZIdC4y/eChKfGZ144Zr149IjLh/XcPZe1P+9FPYiKzMSTsXFU7+x5h8qy1ON8aw5htS2suC4Ylj8eTT9Uk24/PYSW0qQQIYDIWQ8hbuVJIGCzvBAXmjq2diHy0IgpWq+xZPaEaa8u/mv4jPPXIlbPEbz7Zpw5R0+Y8DsMWdf66wJq2yUCZo01NiMLrOoaIWKzuHJJTbpjHlEjAmMh0F/fvFhZ4kzUH3AyCHm0HnG9VmgqDoamu8sHxalTV7YtC41RARpCMt4G/IG65kOGLfkUCNmfVdZOtwTZqTh+6LBjRylpwMGYR8qCiAfQot6OhR1XliKGeZKLQKa+5UacIHYxGjqcNWylktc7DumQttYbVHb4PbVLl3Qnt3ZuX3KS8Q41ITlD1iF9SEf9gXNKscUhK4T6TJIXdbChcgeBvvrmi4RlfZ+xpt3B05EUpddqOXxETeeSTkdyYpaZZLwTh/ZjUVeuzGrjkHUAtV2LYcwLr8IpT7fhlKcbA7CAKmOMAE50+zIavU9g1HoGesoxLmk4i4ZYAm+W5cQHK7vb/hROC4OzimS8C+x7Gps/Zmn5XRzDODU49yRIc2GhFs5A1kuqO9s/mqCSs6gBIABSvgsLNs/ARAhiUQdgQBJVar02pdVnKhO8fWkst7MajoEOIvUgQID8FzysPPHJy8ZDGRIWPUrnzq3tWvSSl6oomwiMItCLNSIyJX6FZxzhMCUPkfGwaqBHvAmdm1ur0guv8FBNpEWTjMdxX1/j/EfRWzvx7RF7Iu33cBhf6A1vwH+fru5s+0U4jKIVSUMAkbkuwJD1D3lQhEeez++K0A8jxOUpHmmIhViSsQ03oof8Gh7U/TmcZQMsu0lMMHghuipy1mlTuheYk5t4EYHAENgwc+5emyeUPySFnB2akbAo74QY9WSeh/VyxJqfFZhzI6KYZGzTUSDkFegdT7eZnMnGQEArtVpKfVN1Z8cNBIoIhAkBzCX/Ow6R+RzOj2CIXBcckyfiThKxHShJxnZQQppMXfOROiX/l3NLNgHbWTIzXIWAZ2LjlvdUr7r//xxIYlYi4BkCffue8U9it4q/QAEWd7GJLBloRNdCt/hUDE8nOsylXfxY0+wiZVikofkavDF/nic9FQHa1uGq/BmljyJwR1MJuZmFCPiOAA6SuQ+xu07EFNVk35VHXCF6xBtALt/GWpBvRrwovplPMi4SajygfwAZH8n5Y/vAYVj6DUx/XV2Vbv+p/VxMSQSCR6C/ofkSJeTV2OLIMLl23VGIXvtUTWf7sXazMB1Dp5dUBzKNLUOYVyovKXOSMuH1GMUdltlsXdXSxa8nqegsa3wQGJh+yszsxClpNJc4Bzk+5fKsJFpvQY/Yn5DCnhXCf8GsWiVg3ju7Fac8qfvQQ35HCdmTkUXrIVDxSqyirE9GgVnKuCNgFnHi7XI/TFV5Gror5IFqx3Sz1uqNslz2jMruJYywVeQDQTIuErDR5L0NLf+FtR0f4fzx2wEECW+UWj+G+eEzS4SX2YhAKBHoa2xG3AF5LJ77CaE0MEijEHMaA9R3Y8HWpUGaEVXdJGMHnsP88QDmkvZ0ICJ2Wc3CDZxXd2NVZ/t1sSscC0QEgECmvulGZVmfQQ+5goC8hQAOq+yr6lzILWElVgqScYnAmWx9DfNPwpvgQsavHgGx8GZ8I96Mr3cAK7MSgdAjgKhd39IyT8hcaW28hW1MSqu5temOJ0PvvJAaSDJ26BgMWy3AsNX8xO9HVHoAUH4VQ9MIK8iLCMQfAYTK/RJOGLsq8YScD3cp7sKzf1H8ve5dCUnGLmDb29iyDA/kTBdEhUJE0QtIsGgD5w5/o7pz0a2hKACNIAI+IdDb2PQFKawvJzkYEBZtpbGNqdEnyGOrhmTsgmsxd/yv2PLwbTyQu7sgLlIisFAro5T6dk1Xx3cjZTiNJQIuIYBgQFfjZfzyJC7mzAf3UPpSnD1+p0twJlYMydgl14OQX8fDuG+S9iHiQeyVQn8X5w/f6BKMFEMEIolAb2Pz13HIxOcSRcgmioDWSzE8XRdJp4XMaJKxSw7paZh3upSpe/GGvIdLIsMtRot1eBLvxOb+z4bbUFpHBPxBAPuQb4em8xOz7UkLxJ4WrdXphdjuxcspAiRjpwhukx97j/8EMn537HvHSm/Gmo27arraPu0ifBRFBCKPAIaszQv52SDkeLet6BVjVOypqs42hrx0qdbGu8K4BJJdMX3Tm94pJlpP4kGcZjdPuNLZOEC18BAuw0M4O1y20xoiEA4E0EPGgkZE54tz64pjUHVWH1+zrANhQnm5gUCcq4sb+BQtAw/ic3gQD4vtg6jFoBZDtTWdS9YXDQ4zEIEEILBmxrypuYrU6tgOV+dPQlW/w1qR9yXAnb4VkWTsMtR9jU04cs1qx4NYcmQuG/1Tl622KQ5zRLmc/she3W2/sZmDyYhAIhHoq5/fLCx9N9qB+AUFYYAPT+o0ydgDWLGyugsPYV2sesdab8Lq6Rtr0u3XegAZRRKB2CHQW998k7TyK6zjU7bCNNXLmKY6KD6FCkdJYlRLwgGosaK/oemcnLR+hjUck8JjlQNLClsYVmELw34OpDArEUgcAjhutQ/HrVbFpuDYV4y24AKEvO2ITZlCUhCSsUeOQFSuNMjY9+MDi46eZaf8Jua0FqdWd7U9ayc50xABIlBAoL9h/nE5oX+DHnIsXsy10n/BsagH07/uI0Aydh/TvMQMhqi0JT8b9S0OGJregtNY7mXcWY8qCsXGHgFMWy3C2O5ZUR+uBhHnpNTfwsKtr8XeaQEUkGTsEei9jXMbpSj/Y+RDZCo9CCKOxVu9R66mWCIwLgI4UGZYSKts3IRhTqD1+jJhHTy1c8HyMJsZVdtIxh56DtucsAcPQ9VRRbkwP/QlzA/9yEOYKJoIxB6Bvobmf8eL+ddxpyJZ2PzBTBii7uQQtVf+iypNeIWHq3Iz9fM+q62y74CMy10VXKqwYieUtdhY3bkwcYdflAov8xGBsRDAYq4tWMw1IUoobW0y0BaAjr+A8Lc8mc0jB5KMPQLWiM1MO30PVTNpqZRWtYdqvBGtdT829t9Y3dlxgzcKKJUIJAuBvvqWbwhLfBG943C8nBcDv9ZvVq0fmi3fuG9TMdmY1j4CJGP7WJWUEvGqQcZyVuSGqpVWmCuO5pBaSZ5iJiLgLQJmiy56x7nILerMARep/4b24ABvEUq2dJKxx/6P5MZ/rbKYILoac8XXewwPxROBRCGA3vGPhCU/HamXc61zAmeWV3d1fDVRzvK5sCRjjwHHtoYP4MEz4TGneKzKPfEK5xTn5LyqpQufdk8oJREBItBT33y8JeWDIOTI7FCQWmdUTs6v6V74OD3oHQIkY++wzUvG0JSVaWgZxsNnQk5H40KQDwxJTY2GsbSSCEQLAbygb4oSGQtOWflSwUjGPsCcaWx+WgvrqEgMTWmt8AZxN4N8+FAxqCKRCPQ2Nv9aCnlaJIKAFELhPoL24OREOsvHQpOMfQAb+41vh5qPRuPhUxlE2rmopqvjfh+goQoikDgEMjPPmq8mpP4HuyzC3/5q7C7W4iaEwPx84hzlc4HDXxl8BsQLdVhR/XH0im/DqurwD1VrvU5mswdWLV38uhdYUCYRSDoCA42tM4e1+hu4eGLosdBinVK5z9V2ddwRelsjbiDJ2AcH9iBYvCX1A5FYxIX4sxiSinbYPh98ShVEwAkCGC3T0Rgp0xmZU2dWdXf8wUl5mXd8BEjG42PkOMW6hjOrh8SElcEv2jAdczV2ebT+P0TZ2ddxoSmACBCBXSIQmaMVld6cGsrtU7li0QDd6S0CJGNv8d0qHUcqbsAw9W4+qStNTWGxxrPoGR9ZmgDmIgJEwA4C6Bn/FZE0Dgz9ok6lB9AeVNopE9M4Q4Bk7Aw/27nx8L2Gh2//UD98hoyVurW6q/0S2wVjQiJABIpGoK+++WfCsi4OfXsgVBpHJjYWXUBmKBoBknHRkJWWAXsLX8Yc0QFhf/ikUA9WdbafXlopmYsIEAE7CKA9eBjtgQkIFN7LvJwL/TymrQ4Pr5HxsSzMVSE+KKMkOM8U0ayK3GtsY4rXVZAKx6S9iGPSDnVVLoURASKwHQKYtnoJe40PjgAZPwoy/gDd5z0CJGPvMc5rwIKNB8B1Hwz1Csr8m7BagWGpmT7BQjVEIJEI4HxjLOi09gt14bHBGK/nC9EetIbazpgYRzL2yZGYM74bqs4PNRnnuZgLNnyqElSTYATQM16HBZ3hjlefD/ihf1yTbv9kgl3lW9FJxj5B3VvX8h8yJS4LPRmboPD91oza3gUbfIKGaohAohBYM2Pe1FyF1S2kVRXqgpuecU5/q7q7/d9DbWdMjCMZ++RIDFP/N6r22cGRsXF1fhx67EvrfoTDvLSmq/2X4yXl34kAESgegb6GpguwfuT7iDsQ7sNY8sPUiFPf2QZ7eXmNAMnYa4RH5GOY+kFsbTo11As2jK2Foan/xtDUOT5BQzVEIFEIYL7YHKnaFNyLuU24C2R8H8h4rs0cTOYAAZKxA/CKyZppnI+Tm0QkTm4CGW/CiupwBygpBnymJQIhQqCvcf5mvJRHIS61eTF/AodEnBAi+GJrCsnYJ9dGJuKO6Rwr3SNF7szq9KLnfIKHaohAIhDIzJ5/tEipxdqyqkNfYLPVUeuXQMbvCb2tMTCQZOyTE3Gm8XNaWof5pM6ZGjyBmF7+LcLgcX+hMySZmwhsh0CmoeV3WsrjQz9dNWq1Ynhcv6owydgnpNEzVpgjig7eWq/UOXlhTffCx32CiGqIQKwRQNQtvNxqEwYz3PuLt/GC1Dpb1dlWHmvHhKRw0SGHkABWihm9s5oaZHnqebwN71FK/mDy6KxU4pWqdNu7g9FPrUQgXgiAjF/BC/kctAPhP9d8FHqcb16Wyx0ytXvR0nh5I3ylIRn74JPexua5Usu7Qr+VYQcstFKrtdSX1nZ2LPABJqogArFFoLe+5SMg4ZulJWsjVcjCVscLsNXxgUjZHUFjScY+OK2vselL2Ff4dbwVT/BBnbsqtH4BWxve665QSvMCAUz110PuPrjNSvjJuCeN3KM/m/q3GffgyOe2P2/Cdz24V2A2ZaMX9iVZJnrFL+JlPHILobB6ZBArOr+KrY7fS7L//Cg7ydgHlDFffDv2GH80Mos2tsHEbHNCQPtHqjsXnuUDVFQxDgLwx0FIcizuf8Zt5h73H/l0c3Xueshchfv1kc8V+HwJ9zMg6l46qTgEehtaHsBqkRPwMh697YI8VrU4ZztITTJ2AJ7drNhj3K+l2O6AbpvxsOyq8DadEqux8OQbWF19i7eKKH1HBEC+5vi643EfN/IZdNSmN2CH2fL2wgg5P0Kv7RoBrJ6+TAn9JWlZe0UVJ61VX01ne01U7Y+K3SRjjz2F4Sks2JDPRmLx1lhvCCDklMidWpnu+LPHkDkSD/Iy27EediRk15nfRM/wHR7JzouF/aaH+yHcZ+A+BXfYF/31wcZ7cd8NbLgvfZvK0Tt73qGiLHU/cNnbyzrjuWws4lLZ4UNrly7p9lxXghWQjD12fk9986VYtPFtPJBm3i7al9ZpzB83hrkQHpPxG/DjP7ldftjcAJln4/4g7qPclu+jvC7o+gXunwEnM8yd6AvD01149usiDwLIGGW4Es/+rZEvS4gLQDL22DkYpnoCm/yPi+J88Y7QgDS2oMIM4aEMbW/NY5XGevsAABl3SURBVDJ2tWcMW08Fxp/DbT7j9CwOozymt2xeQv/m8SMWSvFYJ7JhZMFm9Pfo5o811o9gmurkUIIdE6Pi1ACE0iV4KFfhoXS9NxVUYZXWg9gkOQBC9nS4ttTyhZ2MYV8FynYh7s8qLMKKzobTUj0icECKuBGk/FjJEiKWsa9h/pvYEjgVZTa+jsel9Wt45mfEozDhLAXJ2EO/ZOpbWrQlfxGJoPBF4IBYmZtkTi+r7mo3K3tDdXlMxo6GqWHbZQDrK7jDfY6tNx79NcR+Iu7D1xgJe1lJMRvlDP9BEMX4WWucb67Or+7sWFxMNqa1jwDJ2D5WRafEGcZPaSGPjtUA5CgKeDix5Wl42vot+8o37jN7VENxeUzGJQ1TwyazGvq/cM8JBUg7GGFGIX1qCNCgiy/j/iHIysbh2mFEa+c29dS07m5Vqtexa6ICZTP7u+N15YeqTbz6hSfFq2DhKY1Pz2B4CuyXJetnN9VuKbOWY4g6+gu3xgDNzCNbuewhVd2LQzE36DEZF9Uzhi3TAd1/4uZ5sNvXoWfw6/kgrViEWOyfc9ZBSpU9h6Ae8eoN7/jSpvTmCZusffdctaDfr3Y0SXpIxh55Gyspr8JG/6+CjKO/gGMcjBAur9dS4v9Vdbf9xiM4bYv1mIz/AQIxEa7GvWDH1Uh0zbgJQ5kAs9neh0/OQMlJwDPUW+XGc09/w7zTc9L6uZQROBJxvMKM95wjGhcI4xoEALrBoShm3wkCJGOPqkVvQ/NSbPSf5ZH48IlVeinGOh/GIo9PB2mcx2Q87jA19JutLP+DO3KhDwPw2xroPAWE/McAdDtWmWlo/gmGpT8gpDXDsbCoCFDq79XpdhP9jZfLCJCMXQbUiOtrmDdPiNQvMWwVv7mjsfDSOoN5pQG9RR9X81r7PzyAdlyRQZIxdF8CA7+DO9ZTE+M6obgEZh7ZEPIfissWXOpMXfO+IiV/j2nUSox87RmEJYFF8NN6PQ6QObemq+P+IModZ50kYw+829vYsgyLm2b6tSrGgyKULrKwJ3GtkurjQZz25DEZ73TOGDpNhKU7cJ9WOnBxyln0MLdZAHgcCPnFsKPQ39B0jhLWrVE7gc01XPPL7nQnRsBCuRjRtXIGIIhk7DLoOKEJBypYP8cbc2gDY7hc5J2KU1ots7TsLRPi3KnptmV+6Mw3E96Gw3wbGUPf0VB7H+5pfpUxpnpMxK5DwnoQRc+slnqrXPwSFaxGWJZZmJfcK3+sorqQvWN3qwDJ2F08BbYzrcB2pumJ7BXviKXWQ/hqA4auf4zoPWZLi+eXx2S83ZwxdJltHma4LtaraD132lsKTGzrY0DIWR91jqsKc8Pfxcv1xZgfxgu2xPtlwq9C77gLvWMTxpWXSwiQjF0C0ogxvWKtrXsRj5aN87a4aj2AFdfLsLr8Ziz+uNtFyN8myi8yhp5zofznuFNelieBsu8CGV8UhnLjkJcLwTuXod7OAhlPCYNNobEB8arROz6PvWP3PEIydg9LkHFLBg8thyt3gak5ig1z6ZuEUpdVd3W0uwj9VlEek3F+mBo6LofC73phv02ZG5HuFfP+h9usSB69zWTttpchELPAyBy7aD7N+cehPuhjxPj5wLnNJhauJ8MCrfk6JW9GL3gyRri2O/rUdWVRFqj1m2ENixtFWEnGLnmtr775BoS+/IzzMHgmWvGObapLRoZBDLoaILNeS+rV6C1f7TYpe0zG/wcIf4X7ijyU/oSu+j00/Qn3q7hfxt2FOvamE1cCo32R32y7OwK3mfM+DHeY4qdjFEXOdlLGUvLmSdiS3wIBV+GlmiQ8HoharAOBfLeqc+F14yXl38dHgGQ8Pkbjpuif1bq/Kld/wwO827iJmaCAQGHVdQ8+pNDqxuquld+X4gVz2o+jy2MydmSbzcym12sOV3gAdztIaa3NfI6SAbd6CGjF3YL7EEfC3MlsXmx/4I6oXUvRB7ROyGSziBlufbGQCiNbbBVtw456sxEx+OpqVyxw9IJoW2GME7LaueBcBIf/E45JfDcf4hLBRASv/HCgEjcooR+q7Wp/okRJXq+mLtUsO/l+i0Q/AAEtspPYyzRoYN8L+Tfifv/2eoresuTEzKJCjxarqLdu/vtSljpFS+sKQyiJ3apULHA7pgd4uJ6uSbcf61RU0vOTjB3WAAxtfUyl5C3Oh6cdGhKH7Eqvx+jCejzc5ZYUd8msumva0g4zNGv7imDP+Gco3PfCeO4vsDwDtv0Yd1DHZX4KuJgDNly5Vs9seRe2J12IBVlmYdYwRmf2xGLLZAXmcQXJHYQgroDMiU8gHK6JPMerRARIxiUCZ7KZIa7+YdWDeaZAovA4MD30Wc0xjUJps5WoFsY+rZR4AkT9hCXksqr0gjdQcXd66s8oGfvahysNTSwQEteHdV/taJGA5174eQnuw0srpqNcfwI+JQ2Z98xo3bsstWV2NlV2vCXlCbDiGK1FD0Ju741FhIyQ5sgtb88slR6oSrdxnt0BriRjB+D1NTRjnth6J4enHYBoJ+vI/DL4dwgN6RT8OgWYZ1B530QD2wUREzDXN4zJ56GKc04Wu137yQ/bERtQmseg9xKQjFmQFZkLpHwvjPUd17VnXf7McOdrgziIZAt8nsJe383w+Sb4ehlezhB8Q8L3umKkbjQobcjWLMASG/G6tg7pJ6DO1PAZ9biqmYWZQr9Q09lmpjh4lYAAybgE0EwWEPF/ojH4F9xjD3Nt138b/WVnkWW3cQW9Ys8rb+sba1F+9MFij599zV5+f1OZWN1mUdJCf9W6pw2EbIaMP+GexPElbf7+vWLTrQayHZ+Z0d93eFj47IwPqkmx03ZpNOu2WBfRLmHvMRZt3IJYAl+xZwRTbYsAq24J9QHBAHBSC07mGd3+MFKx0VhtxnwU3trz1xQsRsLSf2lWx67DW+Na/LwGD0E/3uqXI0rXRPw8Cekn4e19ssS5wErIBvxuhmXNXuU90QNYi98rIL4CMvH2j//psTE9Vn7Uu8JIxs/D6DNAxD0lVLdQZfGbkLMvLxVrW68MFQaRMaYQKQv/JI4+1EP4yYws7THSrvRLLVYjQRfanwr8vAnfb0b7shlbDgcxl25i609DnkrkMfvUzZ51c++O2xzugeRyNzRHhQBHo+0SQmUiFO5ZtemOJyODU0gMZdNepCO0OLQ80zBjENm2oHEdRONk5otfVUotkRbmpHBgeiqrl1VuKVsqVy3YXKT47ZL3Vs+dIqeVzVFaH4jzUo+Cs96LSj+9QM5YMiH0HtDnREXs8pYfdRDI+KowlWsBjDkPdcXxtq0wFAr13UQcMy8XvhwRCX1izREXCb3enCXBa0wEDD0K0Y/1Eubl3YSixYE1+o85pZ/Fca5/1f3Wq7W9CwyRlnzpfc6cvGZS2SxVJmZD3Syp5d5og86E4jlg/bX4nIzPMswfl+1qXUfJymOekS15kQ7G6ukjtSW+qLV8SKjc8zVLF71QpAjHydfMmTdjWFsnoVU8DXacDIHmKcTbazh6zkGGLQkRGZt+yVdBwtc7dnjIBIAgZ8Aks8rdl33166+4WQw98FTIUAiBOYWebxZPf55g8etD6OX+pnxIP1q5vP01vy3EaXXvxQjf4ULL01Iid1Vl16KX/LYhyvpIxlH23ojtffUfeqeWFWej4f8ovkJPPR/GrzwGRSu6CCEhY9NMYguN9DQOd9HguJgBhPwliPu2iyJ3KWrwngfFxm/c7oeqaOjAlBaYdxCDYv3YcXCHlsMLqtP3RWpBYDSA9tdKkrG/eHuuLVPXcoC09HlKijPxhlqLISTcnqsNjYIQkLEh4o+AiP87NKB4YAjI2PSKzbGHZj7R9lXKlrPsC6+KteeHclGe7XI7TljoBb+JR7kHRLwIcd7vRSjZvzuWSwGhQSBBzXRoMPfNkEx908nYevU1PMfvAiFjOxBmtWN+hWAB1/kg4ntiDnO+eCBkw5Bf97qsesNm0X/YBV6rCad8bYKTIEIYYsXi5fpb1emFj4bTUFrlFAGSsVMEI5Afq7/nCImGU4vjQMj7xXnRV8BkfAOIODFLf0HGpldsTox66/Lo8Iw1J35aqH+YQ6oScmEYGjHbEfRGPJYblt+qXdZm9tPzijECJOMYO3fHomlx1KT+hn2+j0Ue52AlZFkcQwEGOExtVhgfBTLGKvfkXCBkcxRmk9clXnfRtWL42aIio3ptkifygac5CQmLoMVdVelXL5fiFbMqmlcCECAZJ8DJOxaxt3HuPpYovwmdGKzExlFxMaoFAfWMB4DxQSBiM4eaqAvkcS4K7Pmw/Mav/EgMtpvgZTG98seXiT7g+eDE4eErdl++xOwB5pUgBGLUDCfIay4VtX9204EqZd2MnvJhIJI9XBIbqJiAesYXA787Ay14QMpBHiYQhBk/RlhK767Nt7WJTYjGFctL6wxOLPsDzjH/IhdlxdLDtgpFMrYFU7wT9dQ3XWxJ6ycm9m/U55MD6Bk/ASI2BxEk9gIhP47Ce4rBINYtbfzabbHCGFNFg9iXa15iPlqdbrsrVoVjYYpGgGRcNGTxzGAii/U1zFiMCnEoznY1ITkjeQVAxu+M2qEPbjsWZPxDyLzEbbnbyht65Dmx/tLveKnCV9nAbCVCUL6ISFUteObMji9eCUeAZJzwCrBj8ftmIe52uXggfxpOBGuHz8PUD4OIT0l6FQKx/Bsw+IGXOAw/94pYd+E1XqrwRTawQpxojEAN506vXraI25R8QT0aSiLY3EYD2ChbqfdtnZSZrF5A2M/pUTv71WcyNlHPTOzpRF8gmBMBgKfEknt1hRho+kK0cTZzw1qsrCq3jpSvLOAq6Wh703XrScauQxofgYg1eyuG0j6CYWtzGEYkLh+HqXGiVv40m8RfIOO9AYI5ItKzK7fyTTFw6qWeyfdcsNI9WDB9T0267fOe66KCSCJAMo6k2/wzum/2vHmirOwXGLL25VAApyXzsWd8P8j4TKf2xiE/yNi0I57Oe+reAdF//MejCZfW63M50bpXd9tvolkAWu0HAiRjP1COuI7+Wa37qzK1HISMf+E+s9FHMr4CUHwv4q51zXwQsjk5yLMXNnOEYv/hF7pmry+CtDk/2LylDM+s6Vzyhi86qSSyCJCMI+s6/w3va2xBgyLfEeaFXT4OUx8KMn7Rfy+EUyPI2JDNO7y0LvPOVi/Fuyxb92Pr0tqazrZZLgumuJgiQDKOqWO9KhYI+S+Y+6oHEU30SocTuX6RMcrPZ2cbR4GMzQlCc5z4bmveXRztFBkyVspEz1penW4/yhU8KCQRCLBBSYSb3S1kb33z76RlHYkesqdRl0qx2icyXgkunl6KfXHNAzJ+DmU7zMvyRYKMtTk4Qz1Z3dk+10ssKDt+CJCM4+dTX0rU19iMrSzyxLBF7PJpzvi3IOOTfAE6IkpAxs/A1CO8NDdzwIfBc56uE3NmviFirR9DNK0WZ4KYO4kIkIyT6HWXyowh699C1PvDRMg+9Yz/C2T8KZdgjIUYX8g4xHPGKP9aS4tHTUStWDiUhfAdAZKx75DHSyHOSr4PZHyGZ4u6zPm4iJSAK5u/tRiCrsn4OTXy3Rv4696oyBX4fWjCMQerKbd/zev57GtBxtfEy5POSuMLGf/z2YWesRbr4PNyzNqbOmCmSgZx9+A2C8jKcOeQZpMl5QQl9EhdyB9M6M2l1QahZRt6xBd5o4BSk4CAV9UzCdixjCMIgJBfBiEf4Fpjh2WouDbirsCPf7WESCupn5Ha+qsakq/Iii0bqzuXbEDlzSccvdbMmDd18j3XnTHh6APv9tg5l4KMb/FYR6TE+0LGB5x9p8rlnrS0+mvKAvkOioHKFYvM8ZVbL3MSYX/d6VPU8KTdZIU8QEt9IPbjHY16hEWH4gAkHMY92bXRHJ2vp3+p7lx4dKQcRmNDhwDJOHQuiaZBIOQeROqqKdn6Qg/YNGxlWujH0HjeWbMptUSuWrC5GJkghQ8g/cPF5Ckh7bkg41+VkC+2WfwgY1M3gHuuVBD1Xqfs1rfHlLmQcbGQ+n14lcuClCeV+hKJMm9BA7q6urONi/lKdQrzbUWAZMzK4BoCIOQcCBkd2SIutGhIPYD/eiyV+0pV16K2InK/LanXZDyy6+YUNOheE74TGHzPGwUy3hGU/oamc1Bhr0MjWA1SnloUKRfqbRZEHLodBb47nwpdQYBk7AqMFGIQyNQ176ssmZYWehs2LhO1CRXweaXVVbXpjidtZBk3iddkPGLA4SDj58c1JkEJ/CBjr/Z299Q3H486ez3q4ntAymY9wviX1uvUFquxdsWCN8dPzBREYHwESMbjY8QURSDQM+usD1rlJpa1rNxlNhyqjqHo9VZOnVXV3fGHIsSPm9QnMj4ExPCncY1JUAI/yBhwOhqmHs8dPQ3zj7OkXoR008acU1ZqRWo4e2Hl8iVPjCeTfycCdhEgGdtFiulsI4CgIDdhuPpTO43SpfR6VLqfYAvI5bYFFpHQJzI+CGV7uQizYp80DmQ86iRMt9yipLgYq7Hf1kvWOH3JEvpHVen2a2PvVBbQVwRIxr7CnRxlvQ0tL4GwDt46D1dYoLXBGh46YNqy+1Z6hYRPZDwHZev0qgxRlBsnMjb4D0xvnZmtUK9st8ALdRgjOk8h3vSxUfQRbQ43AiTjcPsn0tYhKMgwGrMyNNQ5rOr6W1Vn27u8LpBPZDwbZLzM67JESX7cyHhrL7mxBS9dsg4vlegQ6y1YsOX1HvYouZ22uogAydhFMClqewTW1LW8J5cSj6IRexxB85v9wMcnMt4fZPy6H+WJig4/yNirBVzjYZxpmH8/9isfJXNDx1d13/fKeOn5dyJQCgIk41JQYx7bCKxpaDq4Mt3xZ9sZHCb0iYxngBhec2hqrLL7QcYAzNMFXGM5xO96HKvKwcLYQoBkbAsmJooKAiTjYDwVdzIOBlVqTRICJOMkeTsBZSUZB+NkknEwuFNrfBAgGcfHlywJECAZB1MNSMbB4E6t8UGAZBwfX7IkXpLxSBzMEZA5Z7xDbSMZ8/EjAs4QIBk7w4+5Q4YAe8bBOIRkHAzu1BofBEjG8fElS+Jlz3h7dNkzZs+YzxsRcBUBkrGrcFJY0AiwZxyMB9gzDgZ3ao0PAiTj+PiSJWHPOLA6QDIODHoqjgkCJOOYOJLFKCDAnnEwNYFkHAzu1BofBEjG8fElS0IyDqwOeE/G5qQRGVgErsCApeLEIEAyToyrk1FQ9oyD8bP3ZJwvF8k4GPdSqw8IkIx9AJkq/EOAZOwf1ttqIhkHgzu1xgcBknF8fMmScJg6sDpAMg4MeiqOCQIk45g4ksUoIMCecTA1gWQcDO7UGh8ESMbx8SVLMhYZbx/O0ilWDPqxA4IkY6dVivmTjgDJOOk1IGblZ884GIeSjIPBnVrjgwDJOD6+ZEk4TB1YHSAZBwY9FccEAZJxTBzJYnDOOMg6UBwZlzxnwK1NQTqZuj1FgGTsKbwU7jcCHKb2G/GtL0HP4KcjPNZOMvYYYIoPDgGScXDYU7MHCJCMPQDVhsjiesY2BO48Ccm4ZOiYMewIkIzD7iHaVxQCJOOi4HItMcnYNSgpKKEIkIwT6vi4FptkHIxnScbB4E6t8UGAZBwfX7IkQIBkHEw1IBkHgzu1xgcBknF8fMmSkIwDqwMk48Cgp+KYIEAyjokjWYwCArvsGZe8m2anyDIC1w6wkIz5BBIBZwiQjJ3hx9whQ4DD1ME4hGQcDO7UGh8ESMbx8SVLwmHqwOoAyTgw6Kk4JgiQjGPiSBZjnGFqdwHiMDWHqd2tUZSWeARIxomvAvECgMPUwfiTPeNgcKfW+CBAMo6PL1kSDlMHVgdIxoFBT8UxQYBkHBNHshgcpg6yDpCMg0SfuuOAAMk4Dl5kGbYiwGHqYCoDyTgY3Kk1PgiQjOPjS5aEw9SB1QGScWDQU3FMECAZx8SRLAaHqYOsAyTjINGn7jggQDKOgxdZhp0PU2t87U0N59amHeocyZgPIRFwhoA3TZUzm5ibCJSMAOeMS4bOUUaSsSP4mJkIeNRvILBEICAESMbBAE8yDgZ3ao0PAuwZx8eXLAkQsEPGLoxec5iaw9R83oiAqwiQjF2Fk8KCRsAOGduzccxjnkjGJGN71YipiIBNBEjGNoFismgg4B4Zj1lekjHJOBoPBK2MDAIk48i4iobaQcBtMt7FkDbJmGRspzoyDRGwjQDJ2DZUTBgFBNwm412UmWRMMo7C40AbI4QAyThCzqKp4yNAMh4fIy9ScDW1F6hSZpIQIBknydsJKCvJOBgnk4yDwZ1a44MAyTg+vmRJgIAfZDwoxIxJUr5GwN9CgGTM2kAEnCFAMnaGH3OHDAE/yBhF5pwx54xDVvNpTtQRIBlH3YO0fzsESMbBVAj2jIPBnVrjgwDJOD6+ZEl8GqZmz/jtVY1kzMePCDhDgGTsDD/mDhkC7BkH4xCScTC4U2t8ECAZx8eXLAl7xoHVAZJxYNBTcUwQIBnHxJEsRgEB9oyDqQkk42Bwp9b4IEAyjo8vWRKScWB1gGQcGPRUHBMESMYxcSSLwZ5xkHWAZBwk+tQdBwRIxnHwIsuwFQEOUwdTGcYi4zEPoyzO3DIpZa64LExNBKKBAMk4Gn6ilTYRIBnbBMrlZOwZuwwoxSUOAZJx4lwe7wKTjIPxL8k4GNypNT4IkIzj40uWBAiQjIOpBiTjYHCn1vgg8P8Bt5lQLqxH4LUAAAAASUVORK5CYII=",
      name: "nhentai",
    },
    {
      link: `https://www.wnacg.com`,
      href: `https://www.wnacg.com/photos-index-aid-${output}.html`,
      logo_url:
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIIAAAAoCAYAAAAyhCJ1AAAACXBIWXMAAAsSAAALEgHS3X78AAAEv0lEQVR42u1by3GjQBBtb+3dxV50xyHgEHAIKAQUAoQgQoAQRAgiBBGCue9F1Eagvbyubc3CMAOMjFzzqijb/Lv7Tc/rHvxyu93Iw+OHd4GHJ4KHJ4KHJ4KHJ4KHJ4KHFj9NT/z95/fQ7hMRVUTUGNwiJaKaiHoHdkRE1C28d4Ct21iMrG3bve4emhFSIkrgPBOUuMYFSmxLcCSiT/xUCXIhoisR3Qy364q2nh36zT4jjDiuxSj/DjgQUUhEGYJ/wP4Mo7KyGJUJiFkZnJsZDKZYc05n+BwnRDgpzorB2v2Tk+EDtqWwbw9ytMJWEzQYybHBtJlOECEAESPN8ZCI8keLxQRbBQdxdpAvehlIl3yeuj/dGBn2sC0R7xZZTAs3kMAUb0T0S7P1RFRojrcgwiK8mK41QCyGCHJHRO/CSRcwssAoOOP3WiFHpaSxk3DGEvA7rZmRWKSVsPtgeW2JDNOMTAeZhYjtJ46TOKfbve7eXU8NZ/Hgq3iRDoGXyEfms1b83YI4W0SrBHYqIOqcrt5DorYQ2Snu02oydC8I1z5CI/R4UI+RHcHonJ4bIWwaCnQuspwNCg1xOuGzcIIUKc6vJ4iyKAa2RHgfKKsaR5VDZjFqWDAdDc+vlH7BSaT/eiBovxyS8NMwK+i01GL/LykfS6Gsz0J1rzVCM4vzA3GdaWYrlGrhDELkyrHUkpRqgKa0xcvE8Svep3CZEm3FonROCRLUcNQRoi+EU1uDDhkHbumIW0ssnsVUVwgh3CqiL4PdnbAjUYIV4/qDps5PDbKYqTbJ+TlzOotzMkKEl69FSiqwL3vyBhP3EbhZFg1MiUwE2VqPQYR8YDQHE/2GqWbQlFiU93rY1BDDUT0CfhSKmo/XI867wehCmZu3VjXsEWgmQu/oOeka9f+AqKxcEyEUWiAQhJBloa4D9kwohJYIMA12yuhOBYlD0TDj68KJtG6jO2z86pwIsok0thp2XZnlX4USQX6j+w6jWr8PBdc09b9tyeA5GuEEIzJRcp2F6Itpe21jW8RCCH44fhaX4cHM66s1+jhzNEIoDEhAhG5khDwjIrpfxOG2+pzpxaTkO+IZY6ubOrEYCYHePpIIkaILuJFTK3Mmp8lmQGdEI/fbCo5038nj9jmLu6ngcimZGBIhIP3qZgo/FiPxWJJNZvcRLqIiCKAJuE6+ijR1sRA3+QrNkrX6CIFo4OQjKZzt70cCw9PkwbCUPq2QTe8Wtx7RR4iE+OmFOuaRM1Z3c/m4RtBdIlOqBome/nUgP0G6RpCkREA7nGfzyZsuI5xBqGokHuUahtt8j8Bpv1acEwpjnrl0DJCGdV8iceXUIUBMiits30NI2373GC5454dXDSndL3eyYyQRnlkwHsVUpXN8IogSK8p9jt7hj2AuE75PNdmkeSQRmgFlKtfVK1rh27kBB4SGI8Nk9bEfGfEhniWXjhNRQYRKg4iXkXmdhQPVwU8N/f/thc6vL189CuYuOs3BHI1wcdCg+hgIkGwgMT7p30c3aoBppMGUCtKoA2avPM9V9vzYve7aLROBl3i39n8DLjWHFNK9ordcrbEUu9dd74wIHt8b/l/ePDwRPDwRPDwRPDwRPDwRPDwRPAzwF1qhg4ubfa+RAAAAAElFTkSuQmCC",
      name: "紳士漫畫",
    },
    {
      link: `https://www.pixiv.net`,
      href: `https://www.pixiv.net/artworks/${output}`,
      logo_url: "https://s.pximg.net/www/images/accounts_index/logo.svg",
      name: "pixiv",
    },
    {
      link: `https://18comic.vip`,
      href: `https://18comic.vip/album/${output}`,
      logo_url: `https://cdn-msp.jmcomic.me/media/logo/new_logo.png?v=${new Date()
        .toISOString()
        .replace(/T.*/, "")
        .replace(/-/g, "")}01`,
      name: "禁漫天堂",
    },
  ];
</script>

<main>
  <div class="container">
    <h1>Mystic Translator</h1>

    <div class="inputs">
      <input
        type="text"
        inputmode="numeric"
        bind:value={input}
        bind:this={input_field}
      />
      <button
        on:click={() => {
          input = "";
          input_field.focus();
        }}
      >
        ⌫
      </button>
    </div>

    <div class="buttons">
      {#each links as link}
        <button
          class="button-link"
          on:click={() => {
            window
              ?.open(output.length ? link.href : link.link, "_blank")
              ?.focus();
          }}
        >
          <img src={link.logo_url} alt={`Logo of ${link.name}`} />
          {link.name}
        </button>
      {/each}
    </div>
  </div>

  <footer>
    For personal use only.<br />
    All trademarks are property of their respective owners.<br />
    Inspired by
    <a href="https://d1074181045.github.io/mystic_transfer/" target="_blank">
      mystic_transfer
    </a>.
  </footer>
</main>

<style lang="scss">
  main {
    height: 100%;
    max-height: 100%;
    margin: 0px auto;
    padding: 0px 5em;
    position: relative;

    display: flex;
    flex-direction: column;
    row-gap: 1em;
    justify-content: space-between;
    text-align: center;
  }

  .container {
    height: 100%;
    max-height: 100%;
    width: 100%;
    margin: 0px auto;
    position: relative;

    display: flex;
    flex-direction: column;
    row-gap: 1em;
    align-items: center;
    justify-content: center;
    text-align: center;
  }

  h1 {
    margin: 0px auto;
  }

  .inputs {
    width: clamp(25ch, 70%, 50ch);
    background-color: #504945;
    border: 1px solid #665c54;
    border-radius: 5px;

    display: flex;
    flex-direction: row;

    * {
      background-color: inherit;

      border: none;
      font-size: 1.25em;
      padding: 5px 10px;
    }

    input {
      width: 100%;
      color: #fbf1c7;

      &:focus {
        outline: none;
      }
    }

    &:focus-within {
      outline: 1px solid #8ec07c;
    }

    button {
      border-radius: 5px;
      color: #bdae93;
      text-align: center;

      &:hover {
        background-color: #665c54;
      }
      &:active {
        background-color: #3c3836;
      }
    }
  }

  .buttons {
    width: 100%;
    display: grid;
    column-gap: 10px;
    row-gap: 10px;

    @media (min-width: 768px) {
      grid-auto-flow: column;
      grid-template-columns: repeat(4, 1fr);
    }
    @media (max-width: 768px) {
      grid-auto-flow: row;
      grid-template-rows: repeat(4, 1fr);
    }
  }

  .button-link {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    background-color: #504945;
    border-color: #665c54;
    color: #fbf1c7;
    border-radius: 10px;

    padding: 5px;
    column-gap: 5px;
    row-gap: 5px;

    img {
      max-height: 40px;
    }
  }

  footer {
    justify-self: stretch;
    margin-bottom: 1em;
    font-size: 0.75em;
  }

  a {
    color: #83a598;
  }
</style>
