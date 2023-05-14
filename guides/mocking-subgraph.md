# Mocking Subgraph

You can mock Subgraph requests much in the same way you would mock an API request.

```typescript
// This is an example mocking a response from Bond Protocol's Subgraph
page.route("https://gateway.thegraph.com/api/17f8839a7d9c7f990a93cb221bf4248b/subgraphs/id/**", async (route, request) => {
    if (request.method() === "POST") {
        const postData = JSON.parse(request.postData() as string);

        if (postData.query.includes("query ListMarkets")) {
            console.log("ListMarkets");

            const resultData = {
                "data": {
                    "markets": [
                        {
                            "id": "1_BondFixedTermCDA_80",
                            "name": "BondFixedTermCDA",
                            "network": "mainnet",
                            "auctioneer": FIXED_TERM_SDA,
                            "teller": FIXED_TERM_TELLER,
                            "marketId": "80",
                            "owner": EXAMPLE_ACCOUNT,
                            "callbackAddress": "0x0000000000000000000000000000000000000000",
                            "capacity": "121000000000000000000000000",
                            "capacityInQuote": false,
                            "chainId": "1",
                            "minPrice": "0",
                            "scale": "1000000000000000000000000000000000000000",
                            "start": null,
                            "conclusion": "1690754400",
                            "payoutToken": {
                                "id": `1_${PAYOUT_TOKEN}`,
                                "address": PAYOUT_TOKEN,
                                "symbol": "IQ",
                                "decimals": "18",
                                "name": "Everipedia IQ"
                            },
                            "quoteToken": {
                                "id": `1_${QUOTE_TOKEN}`,
                                "address": QUOTE_TOKEN,
                                "symbol": "WETH",
                                "decimals": "18",
                                "name": "Wrapped Ether",
                                "lpPair": null,
                                "balancerWeightedPool": null
                            },
                            "vesting": "604800",
                            "vestingType": "fixed-term",
                            "isInstantSwap": false,
                            "hasClosed": false,
                            "totalBondedAmount": "94.145",
                            "totalPayoutAmount": "29999129.676172346866",
                            "creationBlockTimestamp": "1680393791"
                        },
                    ]
                }
            };

            route.fulfill({
                contentType: "application/json",
                body: JSON.stringify(resultData)
            });
        } else {
            route.continue();
        }
    } else {
        route.continue();
    }
});
```
