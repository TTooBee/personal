---
id: cepstrum_dataloader
aliases: []
tags: []
---

# cepstrum_dataloader

## 2025/05/21

### Use T.Spectrogram-2025/05/21
- The original code was using Mel-spectrogram, but it makes the code more difficult to get the
  cepstrum from the speech.
- Weird result..

## 2025/05/22
- Let's just use LFCC to get the cepstrum coefficients, and just do IDCT to get the spectrum.
  - Get Spectrogram from `torchaudio.transforms.Spectrogram`, and get LFCC from `torchaudio.transforms.LFCC`,
    and compare the result.
- The code below is the part that gets formant spectrogram from the LFCC.
```python cepstrum.py Preprocess.cepstrum_analysis
    def cepstrum_analysis(self, x, n_cep=160):
        cep_coef = self.lfcc(x)
        dct_mat = self.lfcc.dct_mat
        spec = 20 * torch.log10(torch.abs(self.feature(x)))

        # inverse dct
        spec_formant_from_lfcc = torch.matmul(
            cep_coef.transpose(-1, -2), dct_mat.transpose(1, 0)
        ).transpose(-1, -2)
```
- The code below is where `self.lfcc` is defined.
```python cepsturm.py Preprocess.__init__
        self.lfcc = torchaudio.transforms.LFCC(
            sample_rate=sample_rate,
            n_filter=257,
            n_lfcc=12,
            speckwargs={
                "n_fft": n_fft,
                "hop_length": hop_length,
                "win_length": win_length,
            },
        )
```
- I need to think about how I will extract the high order cepcstral coefficients.
- `spec` - `spec_formant` = `spec_exc`? Would this be enough?
- Make code to use this without saving the data.
- 13:32:56 : Everything is ready.
  - 13:35:17 : But the program takes too much time. Should I save the data before the training?
