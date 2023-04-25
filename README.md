# Add-on Exposure

Realistically, not all of the trades are calculated by simulation in a daily basis. For the FX trades and SFT trades that cannot be covered by daily simulation, max(MtM,0) + add-on exposure approach is applied for the trades replacement risk calculation. This document goes through how this approach should be implemented based on add-on factor.

Add-on factor tables (profile basis) are uploaded to the production system to monitor the replacement risk. The system could easily pick up the tables for exposure calculation. A complete term profile of add-on factors for FX Forward trades and FX Option trades (including buy/sell domestic currency and sell/buy foreign currency, with gross exposure and collateralized exposure) are stored in production system. Also the system stores the add-on factors of Repo, Reverse Repo, Security Bought & Sold, and Security Borrow and Lending with all currencies, issuer types, credit rating, underlying type and underlying terms.

If add-on factor profiles (in terms of tenor) are missing in the table, it would be approximated by interpolation. Once the add-on factor profile has been obtained for a product, the system proceeds to calculate the exposure profile. The procedure would calculate the stand-alone exposure based on either gross add-on factor collateralized add-on profile. The add-on exposure plus the deal’s MtM (only if it is positive, otherwise it is assumed as 0) is considered as exposure. Then, the proper exposure is added to the counterparty’s profile. After this quick and simple aggregation, the peak of the post-deal exposure is compared with the counterparty’s credit limit. A violation warning would be triggered if the limit is breached.

A counterparty’s exposure limit could be time-dependent and set up in other currencies rather than USD. Also, a counterparty’s exposure profile is time-dependent. In this way, the exposure calculated at each time bucket should be compared with the limit set up at the corresponding time interval. If the exposure is higher than the limit, there should be a limit breach warning triggered. Also, the limit should be compared with the exposure calculated at the related time bucket. If the limit is lower than the exposure, the system should trigger a warning/violation.

Time buckets in the exposure profile that is produced based on add-on factor calculation are determined according to the time buckets in the add-on factor table.

Settlement Risk means the counterparty fails to deliver the terms of a contract at settlement time. It could be the risk associated with default at settlement and can lead to principal risk.

Please note that, the settlement risk only occurs on the trades’ maturities. For instance, the pre-deal settlement risk profile only has values on the time buckets 4m and 12m; the settlement risk is zero at all of other time buckets.


Reference:

https://finpricing.com/aboutus.html
