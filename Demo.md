### 💵 1. Base Wholesale Pricing
  
  • Base Price per Egg: What is the baseline cost per egg before any discounts? (e.g., ₹5.00 or ₹6.00 per egg).
  • Price Adjustment Policy: Is B2B pricing fixed on the website, or is it a starting point for negotiation in the RFQ bidding process?
  ──────
  ### 📊 2. Tiered Wholesale Pricing (Volume Discounts)
  
  We need to confirm the exact quantity brackets and corresponding discount percentages:
  
  • Tier 1 (500 – 999 Eggs): Base Price (No discount).
  • Tier 2 (1,000 – 4,999 Eggs): Is 5% off correct?
  • Tier 3 (5,000+ Eggs): Is 10% off correct?
  • Custom Brackets: Do you have larger tiers (e.g., 10,000+ eggs) with higher discounts?
  ──────
  ### 🏷️ 3. Coupon & Promo Codes
  
  We will build a coupon validation engine. We need to define:
  
  • Active Coupon Codes: The exact code names you want active (e.g.,  FIRSTORDER ,  FESTIVE15 ).
  • Discount Types: For each code, is it:
      • Percentage-based: (e.g., 10% off the total order).
      • Flat-rate: (e.g., flat ₹1,000 off for orders over ₹10,000).
  • Minimum Order Value: The minimum purchase value required to activate the coupon.
  • Usage Constraints: Can a customer use a coupon multiple times, or is it limited to one-time use?
  ──────
  ### 🚫 4. Discount Combination & Restriction Rules
  
  We need to set the boundaries for how discounts interact in the checkout process:
  
  • Coupons + Tiered Pricing: Can a B2B buyer use a coupon code on top of their bulk tiered discount? (Standard business recommendation: No, coupons are blocked on tiered orders to protect profit margins).
  • Points + Coupons (D2C Only): Can a D2C retail customer use their loyalty points and a coupon code on the same purchase?
  • Points Accumulation Rule: Are D2C loyalty points calculated on the original price or the final discounted price after coupons are applied?
