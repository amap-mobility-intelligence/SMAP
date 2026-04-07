<div align="center">
  <h2 style="font-size: 36px; font-weight: bold; color: #333;">
    SMAP: Semantic Route Planning with Map-Grounded Multimodal Alignment
  </h2>
</div>


<div align="center" style="margin-top: 30px;">
  <h3 style="font-size: 24px; font-weight: bold; color: #333;">
    Wenjie Zhang<sup>1,2,* †</sup>, Chen Yang<sup>2,†</sup>, Xin Lu<sup>2</sup>, Zhen Wang<sup>2</sup>, Yue Liu<sup>2,‡</sup>, Bobo Xi<sup>1,§</sup>, Pengbo Zhang<sup>2,§</sup>
  </h3>
</div>

<!-- LOGO -->
<div align="center" style="margin-top: 20px;">
  <div>
    <img src="public/icon/Xidian.png" height="100" alt="Amap" style="margin-right: 20px; display: inline-block;">
    <img src="public/icon/Amap.png" height="100" alt="Xidian" style="margin-right: 20px; display: inline-block;">
  </div>
  <div style="margin-top: 10px; font-size: 14px; color: #666;">
    <sup>1</sup> Xidian University, China &nbsp; <sup>2</sup> Amap, Alibaba Group, China<br>
    *Work done during the internship at Amap, Alibaba Group<br> 
    †Equal contribution &nbsp; ‡Project lead &nbsp; §Corresponding authors
  </div>
</div>

---

## 📖 Framework

<div align="center" style="margin-top: 20px;">
  <img src="public/icon/Framework.png" alt="Framework" width="100%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
</div>

<div align="center" style="margin-top: 15px;">
  <p style="font-size: 12px; font-weight: 500; color: #444;">
    Overview of the SMAP framework. (1) The user query is parsed into structured intents. (2) Candidate POIs are retrieved and filtered based on semantic relevance and spatial coherence. (3) A map tile is rendered with only the candidate POIs. (4) A generator MLLM produces a draft route, which is then verified and refined by a verifier MLLM. (5) The draft—refined pair forms a preference pair for HDPO, aligning the generator toward spatially consistent and preference-aware route generation.
  </p>
</div>

---

## 🚀 News

📢 **[2026-04-07]** SMAP is now **open-source**! Check out the repo and get started. 🔥<br>
📢 **[2026-02-21]** SMAP is accepted by **CVPR 2026**! 🎉
---

## 📌 Citation

If you find our work helpful, please consider citing our paper:

```
@article{zhang2025smap,
  title={SMAP: Semantic Route Planning with Map-Grounded Multimodal Alignment},
  author={Zhang, Wenjie and Yang, Chen and Lu, Xin and Wang, Zhen and Liu, Yue and Xi, Bobo and Zhang, Pengbo},
  journal={CVPR 2026},
  year={2026}
}
```

Your citation helps support our research and further advances the field of semantic route planning. 🚀

---
