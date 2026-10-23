Ulta Blush Shades: Marketing & Product Development Analytics  

A two-notebook analysis of 1,286 blush shades across 77 brands on Ulta, using each shade's swatch color (hex code) and color-season label. It treats every shade as a point in CIELAB color space and answers eight business questions about brand positioning, shade assortment and new-product launch. Results are exported to CSV for a Tableau dashboard.  

Tools: Python (pandas, NumPy, SciPy, scikit-learn, matplotlib, seaborn) and Tableau Public.  

Tableau Public dashboard: [xxxxxx]  

Questions  
Notebook 1: Marketing (01_Marketing_Blush_Analysis.ipynb)  
- Which color seasons does each brand serve, and who under-serves winter and autumn?  
- Which brands have the most distinctive palettes, and which are near-duplicates (distance in LAB space)?  
- What can be built from this data?  

Notebook 2: Product Development (02_Product_Development_Blush_Analysis.ipynb)  
- Where are the whitespace gaps in the market?  
- How redundant are shades within a brand?   
- Which brands have a wide or narrow hue range per product line?  
- What should a new brand launch with?  
- Which brands compete head to head?  

Data  
Ulta_Blush_HEX_Codes.csv: one row per shade (1,405 raw rows).  

Meaning  
- [brand, name, shade, swatch alt] are brand, product name, shade name, swatch description   
- [product url, swatch image url]	are Source links    
- [dominant color 1]	is hex color of the swatch (used for all analysis)  
- [dominant color 2]	is second extracted color (not used, see cleaning)  
- [Season Type] is color-analysis season label (12 types, e.g. "light spring", "true/cool winter")   

The data has no price, ratings, sales or customer information. Findings describe what brands offer, not what sells.  

Cleaning   
- Recovered missing or error-valued product names (127 missing, plus one spreadsheet error cell) from the product URL.
- Removed 119 duplicate swatches (same brand, swatch description and hex), leaving 1,286 shades, 77 brands and 233 products.  
- Dropped dominant color 2: about 42% of its values are near-pure red, which looks like an image-extraction artifact.  
- Converted hex to CIELAB (D65), plus chroma (C) and hue angle (h), with no external color library.  

Method
- Color distance: ΔE76, the Euclidean distance in LAB space. About 2.3 is a just-noticeable difference.  
- Season coverage score (per brand): the average of the share of the 12 season types present and the evenness (normalized Shannon entropy) of the shade mix.  
- Under-serving flag: one-sided binomial test of a brand's winter + autumn share against the market share (24.7%), flagged at p < 0.10.  
- Palette distance (brand pair): average nearest-shade ΔE from each brand's shades to the other's, averaged in both directions. Distinctiveness is a brand's mean distance to all others.  
- Whitespace: a 2-unit grid over the region of LAB space inside the market's convex hull and the sRGB gamut. Points at least 8 ΔE from every existing shade are whitespace, clustered with k-means into 8 launch opportunities.    
- Redundancy: a shade is redundant if another shade in the same brand is within ΔE 5.  
- Hue range: smallest circular arc containing a product line's shades (shades with chroma of at least 8 only), for product lines with at least 4 shades.  
- Launch set: greedy maximum coverage (a set-cover problem), where a launch shade covers market shades within ΔE 8. Two scenarios: market-weighted and season-balanced (weights inversely proportional to season frequency).  
- Rivalry: the geometric mean of the two directions of "share of A's shades with a B shade within ΔE 6".  
- Brand-level analyses use brands with at least 15 shades (37 brands). All thresholds are set at the top of the notebooks and can be changed.  

Key findings:  
Marketing  
- Spring-labeled shades are 63% of the market, with summer at 12.3%, autumn at 15.5% and winter at 9.3%. Winter and autumn together are 24.7%.  
- 5 of 37 brands fall significantly below that: Maybelline, KIKO Milano, bareMinerals, Revolution Beauty and ULTA Beauty Collection. Maybelline has no winter or autumn shades.  
- No brand covers all 12 seasons. NUDESTIX covers 11, and NYX, fwee, DIBS Beauty and ULTA Beauty Collection cover 10.  
- J.Cat Beauty has the most distinctive palette (average ΔE 13.6). fwee and Tarte are the closest brand pair (ΔE 6.5).  

Product development:  
- 3,222 grid points sit at least 8 ΔE from any existing shade. They form 8 opportunities, five of them deep, saturated cool berry, magenta and plum shades. The largest single gap is #b50774.    
- 26% of shades have a near-twin in the same brand. KIKO Milano is highest at 59%.  
- About-face has the widest median product-line hue range (69°), and Stila the narrowest (12°).  
- A 10-shade launch set covers 44% of the market (market-weighted) or 42.5% (season-balanced). Coverage keeps rising to 63% at 20 shades, with diminishing returns.  
- NUDESTIX, KIKO Milano and ColourPop are the closest rivals. The exact top pair changes between ΔE thresholds of 4, 6 and 8.  

Repository contents
.
├── 01_Marketing_Blush_Analysis.ipynb  
├── 02_Product_Development_Blush_Analysis.ipynb  
├── Ulta_Blush_HEX_Codes.csv  
├── tableau_exports/          # created when the notebooks run  
└── README.md  
Tableau exports (tableau_exports/)
