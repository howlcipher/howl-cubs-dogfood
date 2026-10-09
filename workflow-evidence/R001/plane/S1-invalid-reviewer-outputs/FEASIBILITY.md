# Stats API feasibility

FACT: Report generated at 2026-10-08T19:38:56.798512Z; requested observation cutoff 2026-10-08. Query attempts and diagnostics are recorded below.

FACT: This is a research probe, not an implementation of any candidate product.

FACT: Collection is incomplete. Successful observations below do not supply counts for failed or unexecuted queries.

FACT: Name-matched sport IDs used by this run: `{"A": 14, "A+": 13, "AA": 12, "AAA": 11, "MLB": 1}`; minor-sport discovery=`{"11": "Triple-A", "12": "Double-A", "13": "High-A", "14": "Single-A", "16": "Rookie"}`. [Q-8e7729dddc2e](#q-8e7729dddc2eb200ca8a1ea24efbef2e2475afce191b3c6dcfc238d8dac68c10)

INFERENCE: November cohort years mean 2018, 2019, and 2021–2025; prior-season statistics use that calendar year, and MLB outcomes use the following year. The requested design omits 2020. Rule 5 cohorts use December 2015–2025.

FACT: The collector uses sequential requests, a descriptive User-Agent, at least 0.5 seconds between request starts including retries, and at most two retries by default. Raw JSON stays in ignored `data/raw/`; [raw_manifest.json](raw_manifest.json) records exact endpoint, parameters, retrieval UTC timestamp, SHA-256 and bytes for each cached successful JSON response. Cache replay verifies checksums and preserves retrieval times. [probe_results.json](probe_results.json) preserves parsed observations, unclassified counts, event evidence, and run diagnostics.

UNKNOWN: Missing events, silent server truncation, historical coverage, and contemporaneous publication time cannot be established from an empty response or a successful HTTP status. No absence here is a population zero.

## Endpoint experiment design

UNKNOWN: The following are configured requests, not evidence of supported semantics. Only the query ledger records executed queries; cached responses are marked separately from network attempts.

| Label | Purpose | Endpoint under `https://statsapi.mlb.com/api/v1/` | Parameters |
|---|---|---|---|
| UNKNOWN | Discover IDs by returned sport names | `GET sports` | none |
| UNKNOWN | MILBFA and Rule 5 | `GET transactions` | `startDate=YYYY-MM-DD`, `endDate=YYYY-MM-DD`; disjoint 7-day windows; Oct–Dec for MILBFA, Dec for Rule 5 |
| UNKNOWN | Rule 5 return history | `GET transactions` | `playerId=ID`, `startDate=selection date`, `endDate=as-of date` |
| UNKNOWN | Identity join | `GET people/{personId}` | none |
| UNKNOWN | Prior MiLB / following MLB stats | `GET people/{personId}/stats` | `stats=season`, `group=hitting` or `pitching`, `season=YYYY`, `sportIds=discovered ID`, `gameType=R` |
| UNKNOWN | Historical sample | `GET stats` | `stats=season`, `group=hitting` or `pitching`, `season=YYYY`, `sportIds=discovered ID`, `gameType=R`, `limit=1`, `offset=0`, `sortStat=gamesPlayed`, `order=desc` |
| UNKNOWN | Full / date-filtered logs | `GET people/{personId}/stats` | `stats=gameLog`, `group=hitting` or `pitching`, `season=YYYY`, `sportIds=discovered ID`, `gameType=R`; filtered queries add `startDate=YYYY-01-01`, `endDate=YYYY-05-31`, `YYYY-07-15`, or `YYYY-08-31` |

FACT: `draft/{year}` is not queried: no observed schema establishes that it covers Rule 5. No generic draft response is treated as Rule 5 evidence.

INFERENCE: Event hypotheses require explicit descriptions: minor-league free-agency election wording for MILBFA; Rule 5 selection plus major-league phase wording for RULE5. Generic election/selection wording remains ambiguous. No transaction code is assumed. Known total mismatches or responses with at least 1,000 rows trigger interval subdivision; single-day truncation fails clearly. Other silent limits remain unknown.

## A. MILBFA

INFERENCE: **PARTIAL (provisional)**: the implementation can test explicit election descriptions and join player statistics; complete minor-league contract-election cohorts have not been established. Connectivity failure alone is not a NOT FEASIBLE verdict.

| Label | November/December year | Raw rows in queried windows | Explicit events / distinct players | Ambiguous events | Exact duplicates / conflicting event IDs | Missing player IDs | Failed windows | Queries |
|---|---|---|---|---|---|---|---|---|
| FACT | 2018 | 5421 | 0 / 0 | 584 | 5 / 115 | 0 | 0 | [Q-aed565e5a4b5](#q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184), [Q-c03218652ce0](#q-c03218652ce0480f51bbb4db98f0a96aa196af4c9ad1191bc251d04e5357d0f2), [Q-b7664bf52b22](#q-b7664bf52b22c3b3a7919bb8eed42f4fa2e2c402decfe38ce1d7c9c7e49b6e3f), [Q-d90422a81f2f](#q-d90422a81f2fddace1cb164746fb731f9dafb060ef9988aaaa88902172b1193f), [Q-a59a50f57cb1](#q-a59a50f57cb194ea2b8169178097be01dd309cfc1112097fb80ed1b553b57fc5), [Q-39083b93e9bb](#q-39083b93e9bb27ccf9d6691cae21aed2cedfb27808a3880238651c8b3f714513), [Q-6b3dd0bd9171](#q-6b3dd0bd91715b09510b8822d51393d22c5abc1f06d55722e061c8c0645b6efd), [Q-1205574dbaab](#q-1205574dbaabc11ecc91fc75d82dc7d7874428d14bb8f3ee08fa649918359f2e), [Q-062e68ae7ce7](#q-062e68ae7ce756fb6081884a3d8fc0e7396d35a4d62d548277ab17a413f4a744), [Q-6cddebe915c5](#q-6cddebe915c5b15472b20c9f6a3825210a9beb976a0e32e30b625ef9ba232e85), [Q-909b90901260](#q-909b90901260f066053a15b0db50de0ba6b851a8e299a3af421286e1d4b01f32), [Q-b37bb46bc24b](#q-b37bb46bc24bf7ac4d9743ddf810bd6503d84476ff9a18ce41aedd8d7c7c3627), [Q-0422b52dd33e](#q-0422b52dd33ebe5663fbb23768d8dc5d6ca26feed05788f1111e75f449d8df6c), [Q-f3fe4646a49c](#q-f3fe4646a49c81eeb83c48660ef09aef9a62c1e8328472e478e356ced99880ef), [Q-2bc8615ebd52](#q-2bc8615ebd52a9fa45e78d814b146ae00f5b2912e6bcd35a9bf2c953c7deb653) |
| FACT | 2019 | 7176 | 0 / 0 | 574 | 35 / 168 | 0 | 0 | [Q-1c091429c788](#q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b), [Q-d8691442dae2](#q-d8691442dae22b595b47ea6237f8109fd165ee1846effe27940acbf74c2bedf1), [Q-5c26dd158fd8](#q-5c26dd158fd8203d5b42c1ab20dcd5f14ff5868622277d4960585ff09b47215d), [Q-f7927f5b7394](#q-f7927f5b7394c9212970db9bb0503e2f62aaf27aabb7f0c3d1939ae36785f80b), [Q-3fe1e3efe926](#q-3fe1e3efe926b8b0d4a91180a3664eabcf9fac5a837b2e9876c5bce65d60e51c), [Q-a1cfd4e9b00a](#q-a1cfd4e9b00ac62eda09d118666f6ccee80e3fda9824831297d73a8eb18847c3), [Q-38f6349c5d5a](#q-38f6349c5d5a0dcda89a6e3762b14678de11db84904796a4938850f8c969f2d3), [Q-b4b6b728c28f](#q-b4b6b728c28f88058bda44cf83e82e2e0054c03f4ffc2d554044f5eb8ba49748), [Q-11a7b0236a76](#q-11a7b0236a762ab622eebeb2085cc7a6f85f1208166ece85ff44e22125dcfb87), [Q-d8b9ba6fd1c5](#q-d8b9ba6fd1c57fe80b08081f2cd0234a4237a74dded7a5dec2bba20bace5d610), [Q-745cbd00473b](#q-745cbd00473b51d3ebff0026b7a74d1149334e908d9a22a86f9c54483f86f526), [Q-e89b0347f414](#q-e89b0347f414211bb242dd47d96a46652d57f53bc3706222f7f4241a5480c31f), [Q-8d0feb18d7da](#q-8d0feb18d7da8a95577c828d4f3e765388bee0e8e0e8ebc5ab1f91b1ff6ce4b2), [Q-202f55d5d7cc](#q-202f55d5d7cc27b623dd4f91160f0880c2e09e9a250fdddcfed8cdcb4761d08b), [Q-7d06f23eab07](#q-7d06f23eab07cd3c306c1f03c76ce11b61b2a6297ed98e950f16cb92d08248c6), [Q-76461ee862d3](#q-76461ee862d36fbfbe247d2bf135ceb6f947b9d7a197c220187599db35fd42ef), [Q-526966860412](#q-526966860412dea6955ee919df31f82723fa020552df65b465a4719bdba80994), [Q-578085b6d920](#q-578085b6d920f7479d07e049b63d93971245674f0313e42da1af4d2e541fdf62) |
| FACT | 2021 | 8466 | 0 / 0 | 218 | 472 / 1355 | 0 | 1 | [Q-b2746cf151db](#q-b2746cf151db91c75b622e8db60f8ec590143d1ffd88f73f1d19f152e4fba4e6), [Q-c6fa2705661a](#q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c), [Q-5610dd2522bd](#q-5610dd2522bd958d35c96922fc56c5807d9765e024bfb4aee96cb4a389ccc5bd), [Q-33f0b93d09b3](#q-33f0b93d09b321b8335ee9de9bf88697915b670e23ba9111a6ae8c69e73d756b), [Q-78719e29669f](#q-78719e29669fb69ddcc0056b0029f81767188c7e6ddd71f911c93d4f7f5235a4), [Q-54e63e6ceb3f](#q-54e63e6ceb3f560a225dd1e0caac286181a6c6f4f83f7e81cbaa9a3065a5cbac), [Q-4af43c0804ba](#q-4af43c0804baaffd791630a852bd1c94a319c3ce7b9366690cacf5a7ca28a026), [Q-19f18aef3082](#q-19f18aef3082e9082e34016af0a27e666220e1fc39427b5501f1793b918eba32), [Q-d8f6a08d7b3b](#q-d8f6a08d7b3bebf57250d924aa7acae245b76be15097e5bddbad841bad463529), [Q-4cb4f4a24466](#q-4cb4f4a24466b67192b5fa50139cfe5d199f4ffda26bc123e35a4f086ad56921), [Q-8c611c4ce7f0](#q-8c611c4ce7f0064434fa4b90b2089cb6ba8060680de1e7a77779454d330c5350), [Q-72f1a021c250](#q-72f1a021c25024c96826e2695b3cc8cebcd13de9983552ee510633420467dd54), [Q-7195337c6061](#q-7195337c606108ae447c379b8e538d7e05e83294a9b8381cffb8450ced9b0294), [Q-9f566c0058ba](#q-9f566c0058bae739d7925f22e8289f84631cea9867e5adb87305fbe9bc6dab99), [Q-31a281d6c157](#q-31a281d6c157d362d720a39dc0eaf229551a09a57d4e50d642217290ac96ebf3), [Q-6bb7e448dd5b](#q-6bb7e448dd5b3225e032c0827977f1bfe8168658c42b730bda2e8f8041d5b349) |
| FACT | 2022 | 8933 | 0 / 0 | 89 | 156 / 1108 | 0 | 1 | [Q-be7873ee63ca](#q-be7873ee63ca7e03f23f7ae8bd7c3214fb90e27b45882b8a7263f93b1aa40b0c), [Q-995fd6d73263](#q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526), [Q-85e73088c1e7](#q-85e73088c1e7cf54a7ab429a8ad27c58785a0e366e66d3eeb794736040834097), [Q-72bb1eb2ea8d](#q-72bb1eb2ea8dde63b15305a2ddbbc45eceb54c80e98ff579193b62b0d409d06a), [Q-54cfd4c3968c](#q-54cfd4c3968c02de0241ea8db0e056eb424b430e1da43db820006ff90dee4b04), [Q-103d9e875d68](#q-103d9e875d68907fc97b3918fd3c293233636f355fdaf152592455443bfe7661), [Q-c6f8da0d8dc9](#q-c6f8da0d8dc9af9d7be8428a291e5513b9eba7bf6ca58af604d617147cbbd9fe), [Q-8077ce89df62](#q-8077ce89df627ae4311a84ce045ec8269d54ef3d0e789449a5a17668f3ccb894), [Q-f02106d1f842](#q-f02106d1f842c7b5d6de1421a8ef3bfac7c50dd8c3db0f6d49f6964a0b746d38), [Q-e93b9edc20f5](#q-e93b9edc20f556fc51128ebd17fcad94eb0ddea72eb9fb48978c6f7a90bfee5e), [Q-646b38d8ded6](#q-646b38d8ded695a10bff6370d33cd4a392c5b0ae99dfdf8c1c561912aed4c0e1), [Q-5bcab56126c0](#q-5bcab56126c024f4fa9ba66235ccd5ba4527f9a38523cdbf678482788f9eead8), [Q-acacf5d43d22](#q-acacf5d43d22f9d89c9f2ea101ba340c858674ff80ee1524b3ba35e58d9e4f0e), [Q-04ff6b4f9ab8](#q-04ff6b4f9ab81d16b0843993d30a2814042e8bca9936077399aabdc7ca3fe91c), [Q-a5a62f9f6fec](#q-a5a62f9f6fec77900b3f6566dfec38bb1719b0b47d0aceaa58723321385b7b4a) |
| FACT | 2023 | 7090 | 0 / 0 | 215 | 1 / 82 | 0 | 1 | [Q-bb4c91ea405d](#q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281), [Q-39846f85a292](#q-39846f85a292b63175c219f353acf6ca22040d8edf0528788834cc21d3a0bc5f), [Q-0dfdd7c7a988](#q-0dfdd7c7a98862a7b8a3e60b32962b2ec448d45dc686b28e5d691ab282a8cde8), [Q-f2747a84a924](#q-f2747a84a9240cfedc689dc4f034751c7659238d7fa9c11e6ad19d475e574455), [Q-14771f43df16](#q-14771f43df16fd7abe0bfe8c6240c0e84c1fbde3dd84a5bb9f3c43a92b218dd9), [Q-5e6ff3057aa6](#q-5e6ff3057aa6b765c504a04ccdfb5da11d029a6ca81e25b6ef331b05987e8ae4), [Q-61f74abb33c7](#q-61f74abb33c74f178765ebc706f1fcdbd35a5cde11753ccf4c98c82548d5cffe), [Q-91a88687937d](#q-91a88687937d0e01cee428a1396175bc9922cf06604d122b6ec06d28b70ec08f), [Q-2226359382bb](#q-2226359382bb5f93a59d68e1940fda58ab6fd77eb878eb88a1013aa0512c523a), [Q-d1cbe8f01301](#q-d1cbe8f013010981a5938e47e3b3b2ce129501e7e4fa4b49e3dc85676131e989), [Q-fc01607c1582](#q-fc01607c15824dd2d4d6fff869f32bb72f94625a090850b527d14f3e14f7c04a), [Q-817a54a63640](#q-817a54a63640933e98d3912e916e23352dcf656a998e39b5050f361f677b72a2), [Q-23627979a224](#q-23627979a224a34edd659e971da9e0896e828b7a9929eacf752557a613300190), [Q-34e4ae8d9a7e](#q-34e4ae8d9a7e875b02e7acb5beda699d5735a674746ccbe53094f55a66fb6241) |
| FACT | 2024 | 8660 | 0 / 0 | 638 | 0 / 101 | 0 | 0 | [Q-59a34d51aafb](#q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4), [Q-bfcdfef73935](#q-bfcdfef73935567d9c37db9eb966780df55e65678a101bd57b189720c5ed49b6), [Q-a99d8651626e](#q-a99d8651626eb79ec39fe6f15ea85332894604470234455d5a1eaef8790c501e), [Q-9ef9946519d1](#q-9ef9946519d16d2691c912e283912d9083cde0cabc66319cf94ed1728e275921), [Q-68bdca9053a6](#q-68bdca9053a68e1386208afec71e9d567114c7ad82f8d2a3878cb535cf69199e), [Q-6589048a0d21](#q-6589048a0d21505fb218f44f6cc503cdaa846cd94390b53d66b2bde2aa3eb6f1), [Q-637384fcb33c](#q-637384fcb33ca7506d46a6d73f95ffb015ae72f74bb0c74259fbadb6322fbc82), [Q-c8159d2fe7ea](#q-c8159d2fe7ea835df2f6d35ce1cae9901047ea905d483dd34cf94b87c32b1691), [Q-681cc2749c07](#q-681cc2749c078a54e8510fdb5ed55fb5a1c2a8c320571393dc4e6fdf9e9f199e), [Q-9d0b4c235d45](#q-9d0b4c235d457a8570e4fbbda2be47158207335cc72a87561ca973398850e063), [Q-f953f8cb1568](#q-f953f8cb15689162f82ef2ee38cf83c17541f528b509474f8965f0c435d77087), [Q-1ea6aeb6725b](#q-1ea6aeb6725b611dac7b8b6e8e5b5d9741371b0be674bfab95aa0eae9821d4a7), [Q-3ec85a62dfc5](#q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005), [Q-a9a03d66afd2](#q-a9a03d66afd20c182cad4c118f8751f194aa209d85d4a7685e02444091c5686b), [Q-32fbb85bda21](#q-32fbb85bda2116656ea1424c7ab86905fd1d8d70125b62b5e49283e860135d93), [Q-1c2ee5a90e5d](#q-1c2ee5a90e5d8ace4c8665a5eb69d36b34d23caa047890152e3e3d545c7145d9) |
| FACT | 2025 | 6778 | 0 / 0 | 237 | 1 / 310 | 0 | 1 | [Q-a691748e5f44](#q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a), [Q-80eeb9d2b074](#q-80eeb9d2b0740a9af4844b5b5341e9010f860d635911285c75e0e9aefb146066), [Q-8166064bdb7b](#q-8166064bdb7b9628eb2e31ab48718e7fe01525454ef5f7642ff5c360aab9a86b), [Q-9f3e47eef4ad](#q-9f3e47eef4ad0ebdf25495db690b3e0597500fa96388c88fb96986a1227ae130), [Q-d8747801b40c](#q-d8747801b40cd68ba12c2c6278e6147b721280964a4b484d9a38f4943ba53f92), [Q-5d2e5cd378b3](#q-5d2e5cd378b34806ff37b314ecc53a2cd9e9a386198508e7b7e8711c376ce315), [Q-3fdb4502639f](#q-3fdb4502639fdad581001be0afaddda926a4bd5f62984cefb0181848f0064694), [Q-ea68a7549a9a](#q-ea68a7549a9a4fdf726f401969fa740c8000b963b4e1af5bc49dd92eaa1ec9a1), [Q-d30f582e3b5d](#q-d30f582e3b5d75d9078172e70e45134a3cf9881c59d5f3e252dc4c9e49b2bacd), [Q-4de7d93cb687](#q-4de7d93cb68752598a6d76655be814a92cf1325311d65685e3f0a72492237ee4), [Q-85f4fc823916](#q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162), [Q-f0002ef24bf2](#q-f0002ef24bf2472b2f58f86ce53951983191d2b528cea6ed8534200bc233b233), [Q-d7aa234b0bf4](#q-d7aa234b0bf4f7c99babd021778b5e375686dd521aed4cfda3848ff7166a6392), [Q-b292ac78cfec](#q-b292ac78cfec5bcb34d638661860a060e0a5c72b718f0c3c79faea81fffe75f9) |

FACT: 2018: main-window rows=2365; unclassified=1781; adjacent-month explicit events=0. [Q-aed565e5a4b5](#q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184), [Q-c03218652ce0](#q-c03218652ce0480f51bbb4db98f0a96aa196af4c9ad1191bc251d04e5357d0f2), [Q-b7664bf52b22](#q-b7664bf52b22c3b3a7919bb8eed42f4fa2e2c402decfe38ce1d7c9c7e49b6e3f), [Q-d90422a81f2f](#q-d90422a81f2fddace1cb164746fb731f9dafb060ef9988aaaa88902172b1193f), [Q-a59a50f57cb1](#q-a59a50f57cb194ea2b8169178097be01dd309cfc1112097fb80ed1b553b57fc5), [Q-39083b93e9bb](#q-39083b93e9bb27ccf9d6691cae21aed2cedfb27808a3880238651c8b3f714513), [Q-6b3dd0bd9171](#q-6b3dd0bd91715b09510b8822d51393d22c5abc1f06d55722e061c8c0645b6efd), [Q-1205574dbaab](#q-1205574dbaabc11ecc91fc75d82dc7d7874428d14bb8f3ee08fa649918359f2e), [Q-062e68ae7ce7](#q-062e68ae7ce756fb6081884a3d8fc0e7396d35a4d62d548277ab17a413f4a744), [Q-6cddebe915c5](#q-6cddebe915c5b15472b20c9f6a3825210a9beb976a0e32e30b625ef9ba232e85), [Q-909b90901260](#q-909b90901260f066053a15b0db50de0ba6b851a8e299a3af421286e1d4b01f32), [Q-b37bb46bc24b](#q-b37bb46bc24bf7ac4d9743ddf810bd6503d84476ff9a18ce41aedd8d7c7c3627), [Q-0422b52dd33e](#q-0422b52dd33ebe5663fbb23768d8dc5d6ca26feed05788f1111e75f449d8df6c), [Q-f3fe4646a49c](#q-f3fe4646a49c81eeb83c48660ef09aef9a62c1e8328472e478e356ced99880ef), [Q-2bc8615ebd52](#q-2bc8615ebd52a9fa45e78d814b146ae00f5b2912e6bcd35a9bf2c953c7deb653)

FACT: 2018 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | REL | Released | 108 |
| FACT | SC | Status Change | 580 |
| FACT | OUT | Outrighted | 60 |
| FACT | DFA | Declared Free Agency | 584 |
| FACT | SFA | Signed as Free Agent | 131 |
| FACT | CLW | Claimed Off Waivers | 20 |
| FACT | TR | Trade | 70 |
| FACT | ASG | Assigned | 673 |
| FACT | RET | Retired | 7 |
| FACT | DES | Designated for Assignment | 29 |
| FACT | SGN | Signed | 1 |
| FACT | SE | Selected | 98 |
| FACT | NUM | Number Change | 4 |

FACT: Example 2018-10-02, person ID=641501, code=DFA, from=unavailable, to=494: RHP Tyler Danish elected free agency. [Q-aed565e5a4b5](#q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2018-10-02, person ID=572182, code=DFA, from=unavailable, to=422: 2B Darnell Sweeney elected free agency. [Q-aed565e5a4b5](#q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2018-10-02, person ID=544925, code=DFA, from=unavailable, to=552: CF Matthew den Dekker elected free agency. [Q-aed565e5a4b5](#q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2018: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2019: main-window rows=3453; unclassified=2879; adjacent-month explicit events=0. [Q-1c091429c788](#q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b), [Q-d8691442dae2](#q-d8691442dae22b595b47ea6237f8109fd165ee1846effe27940acbf74c2bedf1), [Q-5c26dd158fd8](#q-5c26dd158fd8203d5b42c1ab20dcd5f14ff5868622277d4960585ff09b47215d), [Q-f7927f5b7394](#q-f7927f5b7394c9212970db9bb0503e2f62aaf27aabb7f0c3d1939ae36785f80b), [Q-3fe1e3efe926](#q-3fe1e3efe926b8b0d4a91180a3664eabcf9fac5a837b2e9876c5bce65d60e51c), [Q-a1cfd4e9b00a](#q-a1cfd4e9b00ac62eda09d118666f6ccee80e3fda9824831297d73a8eb18847c3), [Q-38f6349c5d5a](#q-38f6349c5d5a0dcda89a6e3762b14678de11db84904796a4938850f8c969f2d3), [Q-b4b6b728c28f](#q-b4b6b728c28f88058bda44cf83e82e2e0054c03f4ffc2d554044f5eb8ba49748), [Q-11a7b0236a76](#q-11a7b0236a762ab622eebeb2085cc7a6f85f1208166ece85ff44e22125dcfb87), [Q-d8b9ba6fd1c5](#q-d8b9ba6fd1c57fe80b08081f2cd0234a4237a74dded7a5dec2bba20bace5d610), [Q-745cbd00473b](#q-745cbd00473b51d3ebff0026b7a74d1149334e908d9a22a86f9c54483f86f526), [Q-e89b0347f414](#q-e89b0347f414211bb242dd47d96a46652d57f53bc3706222f7f4241a5480c31f), [Q-8d0feb18d7da](#q-8d0feb18d7da8a95577c828d4f3e765388bee0e8e0e8ebc5ab1f91b1ff6ce4b2), [Q-202f55d5d7cc](#q-202f55d5d7cc27b623dd4f91160f0880c2e09e9a250fdddcfed8cdcb4761d08b), [Q-7d06f23eab07](#q-7d06f23eab07cd3c306c1f03c76ce11b61b2a6297ed98e950f16cb92d08248c6), [Q-76461ee862d3](#q-76461ee862d36fbfbe247d2bf135ceb6f947b9d7a197c220187599db35fd42ef), [Q-526966860412](#q-526966860412dea6955ee919df31f82723fa020552df65b465a4719bdba80994), [Q-578085b6d920](#q-578085b6d920f7479d07e049b63d93971245674f0313e42da1af4d2e541fdf62)

FACT: 2019 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 571 |
| FACT | ASG | Assigned | 1701 |
| FACT | DFA | Declared Free Agency | 574 |
| FACT | OUT | Outrighted | 45 |
| FACT | REL | Released | 108 |
| FACT | TR | Trade | 54 |
| FACT | RET | Retired | 20 |
| FACT | SE | Selected | 122 |
| FACT | CLW | Claimed Off Waivers | 41 |
| FACT | SFA | Signed as Free Agent | 173 |
| FACT | DES | Designated for Assignment | 32 |
| FACT | LON | Loan | 5 |
| FACT | RTN | Returned | 2 |
| FACT | NUM | Number Change | 1 |
| FACT | SGN | Signed | 4 |

FACT: Example 2019-10-01, person ID=543219, code=DFA, from=unavailable, to=568: LHP Sean Gilmartin elected free agency. [Q-1c091429c788](#q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2019-10-01, person ID=606959, code=DFA, from=unavailable, to=484: RHP Rookie Davis elected free agency. [Q-1c091429c788](#q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2019-10-01, person ID=571918, code=DFA, from=unavailable, to=588: 3B Deven Marrero elected free agency. [Q-1c091429c788](#q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2019: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2021: main-window rows=2013; unclassified=1795; adjacent-month explicit events=0. [Q-b2746cf151db](#q-b2746cf151db91c75b622e8db60f8ec590143d1ffd88f73f1d19f152e4fba4e6), [Q-c6fa2705661a](#q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c), [Q-5610dd2522bd](#q-5610dd2522bd958d35c96922fc56c5807d9765e024bfb4aee96cb4a389ccc5bd), [Q-33f0b93d09b3](#q-33f0b93d09b321b8335ee9de9bf88697915b670e23ba9111a6ae8c69e73d756b), [Q-78719e29669f](#q-78719e29669fb69ddcc0056b0029f81767188c7e6ddd71f911c93d4f7f5235a4), [Q-54e63e6ceb3f](#q-54e63e6ceb3f560a225dd1e0caac286181a6c6f4f83f7e81cbaa9a3065a5cbac), [Q-4af43c0804ba](#q-4af43c0804baaffd791630a852bd1c94a319c3ce7b9366690cacf5a7ca28a026), [Q-19f18aef3082](#q-19f18aef3082e9082e34016af0a27e666220e1fc39427b5501f1793b918eba32), [Q-d8f6a08d7b3b](#q-d8f6a08d7b3bebf57250d924aa7acae245b76be15097e5bddbad841bad463529), [Q-4cb4f4a24466](#q-4cb4f4a24466b67192b5fa50139cfe5d199f4ffda26bc123e35a4f086ad56921), [Q-8c611c4ce7f0](#q-8c611c4ce7f0064434fa4b90b2089cb6ba8060680de1e7a77779454d330c5350), [Q-72f1a021c250](#q-72f1a021c25024c96826e2695b3cc8cebcd13de9983552ee510633420467dd54), [Q-7195337c6061](#q-7195337c606108ae447c379b8e538d7e05e83294a9b8381cffb8450ced9b0294), [Q-9f566c0058ba](#q-9f566c0058bae739d7925f22e8289f84631cea9867e5adb87305fbe9bc6dab99), [Q-31a281d6c157](#q-31a281d6c157d362d720a39dc0eaf229551a09a57d4e50d642217290ac96ebf3), [Q-6bb7e448dd5b](#q-6bb7e448dd5b3225e032c0827977f1bfe8168658c42b730bda2e8f8041d5b349)

FACT: 2021 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1068 |
| FACT | SFA | Signed as Free Agent | 149 |
| FACT | ASG | Assigned | 269 |
| FACT | REL | Released | 63 |
| FACT | TR | Trade | 47 |
| FACT | DFA | Declared Free Agency | 218 |
| FACT | OUT | Outrighted | 44 |
| FACT | CLW | Claimed Off Waivers | 11 |
| FACT | SE | Selected | 110 |
| FACT | RET | Retired | 3 |
| FACT | LON | Loan | 5 |
| FACT | RTN | Returned | 2 |
| FACT | DES | Designated for Assignment | 20 |
| FACT | NUM | Number Change | 4 |

FACT: Example 2021-10-03, person ID=701029, code=DFA, from=unavailable, to=579: LHP Guillermo Arvizu elected free agency. [Q-c6fa2705661a](#q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2021-10-04, person ID=600301, code=DFA, from=unavailable, to=533: 2B Taylor Motter elected free agency. [Q-c6fa2705661a](#q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2021-10-04, person ID=592644, code=DFA, from=unavailable, to=568: RHP Adam Plutko elected free agency. [Q-c6fa2705661a](#q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2021: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2022: main-window rows=2153; unclassified=2064; adjacent-month explicit events=0. [Q-be7873ee63ca](#q-be7873ee63ca7e03f23f7ae8bd7c3214fb90e27b45882b8a7263f93b1aa40b0c), [Q-995fd6d73263](#q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526), [Q-85e73088c1e7](#q-85e73088c1e7cf54a7ab429a8ad27c58785a0e366e66d3eeb794736040834097), [Q-72bb1eb2ea8d](#q-72bb1eb2ea8dde63b15305a2ddbbc45eceb54c80e98ff579193b62b0d409d06a), [Q-54cfd4c3968c](#q-54cfd4c3968c02de0241ea8db0e056eb424b430e1da43db820006ff90dee4b04), [Q-103d9e875d68](#q-103d9e875d68907fc97b3918fd3c293233636f355fdaf152592455443bfe7661), [Q-c6f8da0d8dc9](#q-c6f8da0d8dc9af9d7be8428a291e5513b9eba7bf6ca58af604d617147cbbd9fe), [Q-8077ce89df62](#q-8077ce89df627ae4311a84ce045ec8269d54ef3d0e789449a5a17668f3ccb894), [Q-f02106d1f842](#q-f02106d1f842c7b5d6de1421a8ef3bfac7c50dd8c3db0f6d49f6964a0b746d38), [Q-e93b9edc20f5](#q-e93b9edc20f556fc51128ebd17fcad94eb0ddea72eb9fb48978c6f7a90bfee5e), [Q-646b38d8ded6](#q-646b38d8ded695a10bff6370d33cd4a392c5b0ae99dfdf8c1c561912aed4c0e1), [Q-5bcab56126c0](#q-5bcab56126c024f4fa9ba66235ccd5ba4527f9a38523cdbf678482788f9eead8), [Q-acacf5d43d22](#q-acacf5d43d22f9d89c9f2ea101ba340c858674ff80ee1524b3ba35e58d9e4f0e), [Q-04ff6b4f9ab8](#q-04ff6b4f9ab81d16b0843993d30a2814042e8bca9936077399aabdc7ca3fe91c), [Q-a5a62f9f6fec](#q-a5a62f9f6fec77900b3f6566dfec38bb1719b0b47d0aceaa58723321385b7b4a)

FACT: 2022 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1104 |
| FACT | RET | Retired | 5 |
| FACT | ASG | Assigned | 569 |
| FACT | DFA | Declared Free Agency | 89 |
| FACT | DEC | Deceased | 2 |
| FACT | TR | Trade | 49 |
| FACT | OUT | Outrighted | 25 |
| FACT | CLW | Claimed Off Waivers | 11 |
| FACT | REL | Released | 48 |
| FACT | SFA | Signed as Free Agent | 83 |
| FACT | LON | Loan | 9 |
| FACT | SE | Selected | 101 |
| FACT | DES | Designated for Assignment | 53 |
| FACT | CU | Recalled | 1 |
| FACT | NUM | Number Change | 3 |
| FACT | RTN | Returned | 1 |

FACT: Example 2022-10-06, person ID=605446, code=DFA, from=unavailable, to=1960: RHP Dereck Rodriguez elected free agency. [Q-995fd6d73263](#q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2022-10-06, person ID=621006, code=DFA, from=unavailable, to=568: SS Richie Martin elected free agency. [Q-995fd6d73263](#q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2022-10-06, person ID=664042, code=DFA, from=unavailable, to=568: RHP Travis Lakins Sr. elected free agency. [Q-995fd6d73263](#q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2022: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2023: main-window rows=2298; unclassified=2083; adjacent-month explicit events=0. [Q-bb4c91ea405d](#q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281), [Q-39846f85a292](#q-39846f85a292b63175c219f353acf6ca22040d8edf0528788834cc21d3a0bc5f), [Q-0dfdd7c7a988](#q-0dfdd7c7a98862a7b8a3e60b32962b2ec448d45dc686b28e5d691ab282a8cde8), [Q-f2747a84a924](#q-f2747a84a9240cfedc689dc4f034751c7659238d7fa9c11e6ad19d475e574455), [Q-14771f43df16](#q-14771f43df16fd7abe0bfe8c6240c0e84c1fbde3dd84a5bb9f3c43a92b218dd9), [Q-5e6ff3057aa6](#q-5e6ff3057aa6b765c504a04ccdfb5da11d029a6ca81e25b6ef331b05987e8ae4), [Q-61f74abb33c7](#q-61f74abb33c74f178765ebc706f1fcdbd35a5cde11753ccf4c98c82548d5cffe), [Q-91a88687937d](#q-91a88687937d0e01cee428a1396175bc9922cf06604d122b6ec06d28b70ec08f), [Q-2226359382bb](#q-2226359382bb5f93a59d68e1940fda58ab6fd77eb878eb88a1013aa0512c523a), [Q-d1cbe8f01301](#q-d1cbe8f013010981a5938e47e3b3b2ce129501e7e4fa4b49e3dc85676131e989), [Q-fc01607c1582](#q-fc01607c15824dd2d4d6fff869f32bb72f94625a090850b527d14f3e14f7c04a), [Q-817a54a63640](#q-817a54a63640933e98d3912e916e23352dcf656a998e39b5050f361f677b72a2), [Q-23627979a224](#q-23627979a224a34edd659e971da9e0896e828b7a9929eacf752557a613300190), [Q-34e4ae8d9a7e](#q-34e4ae8d9a7e875b02e7acb5beda699d5735a674746ccbe53094f55a66fb6241)

FACT: 2023 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1046 |
| FACT | SFA | Signed as Free Agent | 129 |
| FACT | ASG | Assigned | 666 |
| FACT | REL | Released | 70 |
| FACT | DFA | Declared Free Agency | 215 |
| FACT | DES | Designated for Assignment | 19 |
| FACT | OUT | Outrighted | 15 |
| FACT | CLW | Claimed Off Waivers | 10 |
| FACT | DR | Drafted | 14 |
| FACT | TR | Trade | 47 |
| FACT | SE | Selected | 60 |
| FACT | LON | Loan | 6 |
| FACT | RTN | Returned | 1 |

FACT: Example 2023-10-02, person ID=501303, code=DFA, from=unavailable, to=431: SS Ehire Adrianza elected free agency. [Q-bb4c91ea405d](#q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2023-10-02, person ID=607359, code=DFA, from=unavailable, to=400: RHP Spencer Patton elected free agency. [Q-bb4c91ea405d](#q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2023-10-02, person ID=594577, code=DFA, from=unavailable, to=494: RHP Mike Mayers elected free agency. [Q-bb4c91ea405d](#q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2023: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2024: main-window rows=3853; unclassified=3215; adjacent-month explicit events=0. [Q-59a34d51aafb](#q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4), [Q-bfcdfef73935](#q-bfcdfef73935567d9c37db9eb966780df55e65678a101bd57b189720c5ed49b6), [Q-a99d8651626e](#q-a99d8651626eb79ec39fe6f15ea85332894604470234455d5a1eaef8790c501e), [Q-9ef9946519d1](#q-9ef9946519d16d2691c912e283912d9083cde0cabc66319cf94ed1728e275921), [Q-68bdca9053a6](#q-68bdca9053a68e1386208afec71e9d567114c7ad82f8d2a3878cb535cf69199e), [Q-6589048a0d21](#q-6589048a0d21505fb218f44f6cc503cdaa846cd94390b53d66b2bde2aa3eb6f1), [Q-637384fcb33c](#q-637384fcb33ca7506d46a6d73f95ffb015ae72f74bb0c74259fbadb6322fbc82), [Q-c8159d2fe7ea](#q-c8159d2fe7ea835df2f6d35ce1cae9901047ea905d483dd34cf94b87c32b1691), [Q-681cc2749c07](#q-681cc2749c078a54e8510fdb5ed55fb5a1c2a8c320571393dc4e6fdf9e9f199e), [Q-9d0b4c235d45](#q-9d0b4c235d457a8570e4fbbda2be47158207335cc72a87561ca973398850e063), [Q-f953f8cb1568](#q-f953f8cb15689162f82ef2ee38cf83c17541f528b509474f8965f0c435d77087), [Q-1ea6aeb6725b](#q-1ea6aeb6725b611dac7b8b6e8e5b5d9741371b0be674bfab95aa0eae9821d4a7), [Q-3ec85a62dfc5](#q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005), [Q-a9a03d66afd2](#q-a9a03d66afd20c182cad4c118f8751f194aa209d85d4a7685e02444091c5686b), [Q-32fbb85bda21](#q-32fbb85bda2116656ea1424c7ab86905fd1d8d70125b62b5e49283e860135d93), [Q-1c2ee5a90e5d](#q-1c2ee5a90e5d8ace4c8665a5eb69d36b34d23caa047890152e3e3d545c7145d9)

FACT: 2024 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1889 |
| FACT | ASG | Assigned | 833 |
| FACT | DFA | Declared Free Agency | 638 |
| FACT | REL | Released | 104 |
| FACT | SFA | Signed as Free Agent | 152 |
| FACT | OUT | Outrighted | 54 |
| FACT | TR | Trade | 55 |
| FACT | CLW | Claimed Off Waivers | 19 |
| FACT | SE | Selected | 70 |
| FACT | DES | Designated for Assignment | 22 |
| FACT | NUM | Number Change | 3 |
| FACT | LON | Loan | 7 |
| FACT | RET | Retired | 4 |
| FACT | CU | Recalled | 1 |
| FACT | SGN | Signed | 1 |
| FACT | RTN | Returned | 1 |

FACT: Example 2024-10-01, person ID=641432, code=DFA, from=unavailable, to=561: 1B Willie Calhoun elected free agency. [Q-59a34d51aafb](#q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2024-10-01, person ID=665506, code=DFA, from=unavailable, to=564: CF Cristian Pache elected free agency. [Q-59a34d51aafb](#q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2024-10-01, person ID=642731, code=DFA, from=unavailable, to=105: 2B Thairo Estrada elected free agency. [Q-59a34d51aafb](#q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2024-12-11, person ID=691634, code=R5M, from=507, to=494: Chicago White Sox purchased contract of RHP Joseph Yabbour from New York Mets in the Rule 5 Draft, AAA Phase. [Q-3ec85a62dfc5](#q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2024-12-11, person ID=666768, code=R5, from=146, to=144: Atlanta Braves purchased contract of RHP Anderson Pilar from Miami Marlins in the Rule 5 Draft. [Q-3ec85a62dfc5](#q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2024-12-11, person ID=691711, code=R5M, from=453, to=235: St. Louis Cardinals purchased contract of RHP Jawilme Ramirez from New York Mets in the Rule 5 Draft, AAA Phase. [Q-3ec85a62dfc5](#q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: 2024: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2025: main-window rows=1627; unclassified=1390; adjacent-month explicit events=0. [Q-a691748e5f44](#q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a), [Q-80eeb9d2b074](#q-80eeb9d2b0740a9af4844b5b5341e9010f860d635911285c75e0e9aefb146066), [Q-8166064bdb7b](#q-8166064bdb7b9628eb2e31ab48718e7fe01525454ef5f7642ff5c360aab9a86b), [Q-9f3e47eef4ad](#q-9f3e47eef4ad0ebdf25495db690b3e0597500fa96388c88fb96986a1227ae130), [Q-d8747801b40c](#q-d8747801b40cd68ba12c2c6278e6147b721280964a4b484d9a38f4943ba53f92), [Q-5d2e5cd378b3](#q-5d2e5cd378b34806ff37b314ecc53a2cd9e9a386198508e7b7e8711c376ce315), [Q-3fdb4502639f](#q-3fdb4502639fdad581001be0afaddda926a4bd5f62984cefb0181848f0064694), [Q-ea68a7549a9a](#q-ea68a7549a9a4fdf726f401969fa740c8000b963b4e1af5bc49dd92eaa1ec9a1), [Q-d30f582e3b5d](#q-d30f582e3b5d75d9078172e70e45134a3cf9881c59d5f3e252dc4c9e49b2bacd), [Q-4de7d93cb687](#q-4de7d93cb68752598a6d76655be814a92cf1325311d65685e3f0a72492237ee4), [Q-85f4fc823916](#q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162), [Q-f0002ef24bf2](#q-f0002ef24bf2472b2f58f86ce53951983191d2b528cea6ed8534200bc233b233), [Q-d7aa234b0bf4](#q-d7aa234b0bf4f7c99babd021778b5e375686dd521aed4cfda3848ff7166a6392), [Q-b292ac78cfec](#q-b292ac78cfec5bcb34d638661860a060e0a5c72b718f0c3c79faea81fffe75f9)

FACT: 2025 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 384 |
| FACT | SC | Status Change | 665 |
| FACT | SFA | Signed as Free Agent | 115 |
| FACT | DFA | Declared Free Agency | 237 |
| FACT | REL | Released | 46 |
| FACT | RET | Retired | 3 |
| FACT | DES | Designated for Assignment | 35 |
| FACT | TR | Trade | 45 |
| FACT | SE | Selected | 77 |
| FACT | RTN | Returned | 1 |
| FACT | CLW | Claimed Off Waivers | 5 |
| FACT | OUT | Outrighted | 6 |
| FACT | LON | Loan | 5 |
| FACT | SGN | Signed | 3 |

FACT: Example 2025-10-01, person ID=663765, code=DFA, from=unavailable, to=2310: RHP Jake Woodford elected free agency. [Q-a691748e5f44](#q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2025-10-01, person ID=622379, code=DFA, from=unavailable, to=568: RHP Luis F. Castillo elected free agency. [Q-a691748e5f44](#q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2025-10-01, person ID=682175, code=DFA, from=unavailable, to=529: LHP Joe Jacques elected free agency. [Q-a691748e5f44](#q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2025-12-10, person ID=685112, code=R5, from=146, to=143: Philadelphia Phillies purchased contract of RHP Zach McCambley  in the Rule 5 Draft. [Q-85f4fc823916](#q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2025-12-10, person ID=696521, code=R5M, from=158, to=142: Minnesota Twins purchased contract of RF Garrett Spain  in the Rule 5 Draft, AAA Phase. [Q-85f4fc823916](#q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2025-12-10, person ID=689450, code=R5M, from=115, to=116: Detroit Tigers purchased contract of RHP Luke Taggart  in the Rule 5 Draft, AAA Phase. [Q-85f4fc823916](#q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: 2025: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

UNKNOWN: A description may omit contract status; releases, generic free agency, and major-league free agency cannot safely identify this population. October/December counts are reporting-window checks, not additions to the November cohort. Event dates do not establish first publication time.

INFERENCE: Prior-year stats and a known election could inform an offseason decision. Following-season MLB games, PA, and pitching outs are outcome labels available only later. Current people records and corrected stats can introduce hindsight. Empty MLB stats do not prove zero appearances. Hitting and pitching games are kept separate; their sum would double-count two-way appearances. Only regular-season (`gameType=R`) outcomes are tested.

## B. RULE5

INFERENCE: **PARTIAL (provisional)**: explicit phase and club evidence could support observed selections, but neither complete MLB-phase lists nor complete return histories have been established.

| Label | November/December year | Raw rows in queried windows | Explicit events / distinct players | Ambiguous events | Exact duplicates / conflicting event IDs | Missing player IDs | Failed windows | Queries |
|---|---|---|---|---|---|---|---|---|
| FACT | 2015 | 1663 | 0 / 0 | 0 | 1 / 68 | 0 | 0 | [Q-d9c8e40ee22d](#q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b), [Q-233d80c8096f](#q-233d80c8096f1f7ef00061c24a09259889c2e2d115436ee935ca298126aee34c), [Q-19e72096bdc0](#q-19e72096bdc0bede16287aa14b1b1d6ac6dc1bd61adaeb6645bdde0e403b686d), [Q-43e9763d2d3f](#q-43e9763d2d3f89e3f3fbfe9b517cdadf2a577cc09afe70cdadb0f0a58d3dd8dd), [Q-5283c1735911](#q-5283c1735911819cc64fa9125120c494ac86befd74fdd54fe39542932b0f1ecc) |
| FACT | 2016 | 1378 | 0 / 0 | 0 | 0 / 39 | 0 | 0 | [Q-0a2b75bb513f](#q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3), [Q-c390ddd9fa88](#q-c390ddd9fa8850b2b4f5169fa77734af1858d07ba2f31be2e05a8026974fe5dd), [Q-3d232100e262](#q-3d232100e262f518c01cba3ce51f8b50363b66d801d582141396b1f9f57bfc06), [Q-f4871493ed80](#q-f4871493ed80a598a5cb41672d1f73f4565bdb0ea257e74eb2035919afca37a2), [Q-c3d5e6f973e7](#q-c3d5e6f973e753ab956046f114b0de198208929d7e70f23ba2382ab081773653) |
| FACT | 2017 | 1311 | 0 / 0 | 0 | 0 / 65 | 0 | 0 | [Q-94ea0454e66f](#q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993), [Q-4e5dc646ff22](#q-4e5dc646ff22f2d66c270f49bcbb62a0f2d5efe14fde796499e7e9cd9683a16d), [Q-2e696bb0604b](#q-2e696bb0604b14d5cc0d616c041d88e808c6d3e422d8851dd31367745c42786a), [Q-14ac1c3b2718](#q-14ac1c3b2718a561b559acdb839a641418de2b10c6df2f8b626d8f1402105d8a), [Q-8814b82fd0b2](#q-8814b82fd0b2f67c21f783a9f6b27b6eec037788220f4ef6bd7efb984bb2acb9) |
| FACT | 2018 | 1253 | 0 / 0 | 0 | 0 / 59 | 0 | 0 | [Q-f879c3f65b13](#q-f879c3f65b13541057d672ed63a5b9f8b1801322341a2761dbe21060a61ff3fe), [Q-4bbef26ec32f](#q-4bbef26ec32f48840f279b8326e3212c62d41766e3ef18fc936713b1a2103359), [Q-d8a617158995](#q-d8a6171589959c2cdd77e8cca8e42e05ae4c3d8809d5aa82ceb1a735e160ff88), [Q-70c75d3b5e7e](#q-70c75d3b5e7ed79b7c1b7230b70f8a9bac75ee759d7138adbf03efe251bb741c), [Q-afb5d1a97b50](#q-afb5d1a97b505004cbe28ab0cd10f607ee2fefa3d7db7bb8123336c6a9a7ab1b) |
| FACT | 2019 | 1538 | 0 / 0 | 0 | 7 / 54 | 0 | 0 | [Q-27d37c5e2f42](#q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a), [Q-e99ef1dee109](#q-e99ef1dee1093a44b99bd7015739384610d246125a4be8a5596548cd4b8ba8f2), [Q-4b325fb28d97](#q-4b325fb28d97bd86f5e7f44cb9e5ee324f97a8f19129aaea2b8e371982da9719), [Q-c0d97ed051c2](#q-c0d97ed051c2352f9461be18c5d9b7abed7b1d3a54a3dbbcbdf685bfe060a925), [Q-4aebfb6dd7ec](#q-4aebfb6dd7ec76d8e957293bd8acb9acb2dbf206a13075f3beb6941442953ae7) |
| FACT | 2020 | 2604 | 0 / 0 | 0 | 26 / 95 | 0 | 0 | [Q-2c5d9ad70bdc](#q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70), [Q-e04a4e8d49ce](#q-e04a4e8d49ce851fa6c27ff10057930cd64f6d0aa2c291ed695f1a55f6e2bbda), [Q-01ecfec45634](#q-01ecfec456342435b221f8f68f7ffa850a3ab1fefe9113d30e97314b411c6977), [Q-805d274a19a9](#q-805d274a19a9f49fe09b06fe9188983bc8aa7d9486295a7fa0d3454e1de73515), [Q-fd397dc3e730](#q-fd397dc3e730ee27a4a1d12d9ae6ea437095113c36949ef432876d50f1700567) |
| FACT | 2021 | 1803 | 0 / 0 | 0 | 23 / 255 | 0 | 0 | [Q-e4d3ae0895ed](#q-e4d3ae0895edecb34cef8a6548bb80e7b7e8863cea9301c052fe2a67c4223bc4), [Q-0e4cd9ba2efc](#q-0e4cd9ba2efcf3ba0699665547a13e0d719fec4944e69b2b95609a228930366d), [Q-8a05cb012f01](#q-8a05cb012f015939fc8e42057023e43a057e665cebe3817f2870ee439f6c7d64), [Q-ba0156e53a9a](#q-ba0156e53a9a30df2ed1e2f1350193915f08de5e7de4b9225353ce50bb160464), [Q-cc532385b8b5](#q-cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52) |
| FACT | 2022 | 2793 | 0 / 0 | 0 | 48 / 327 | 0 | 0 | [Q-2fbb208a9d37](#q-2fbb208a9d374dc3045e6bb120b700c36d43cd10d5c71caa9f4fa1c80b9a0222), [Q-01b69e34c554](#q-01b69e34c5545e8a130038b64f2cdf4a185e8e44f0d9bef4c5b29f1f14ea6eb0), [Q-18f23a4cf0b6](#q-18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b), [Q-2a9b6f78ee40](#q-2a9b6f78ee4062ba791fd3dce3a1bf5a22f39217c729712572afd2971321f86c), [Q-6f26709b0806](#q-6f26709b0806358cb39c2a8964aec6bc14c22ec6b573d071387b483af9d84a81) |
| FACT | 2023 | 2002 | 0 / 0 | 0 | 0 / 48 | 0 | 0 | [Q-c2b708962c1a](#q-c2b708962c1aa44d3a7cce177a83cc6c2488ed63a32347ebcd5865a42ed813a3), [Q-24af1ee31dcf](#q-24af1ee31dcfa11e4d414e44b7c2464693e9e3182528045ba4f62c6773a1af5b), [Q-dca79c9efd5c](#q-dca79c9efd5c5232559662e41bed8fe0f77a7c152a464c17e17c71bd630ab38a), [Q-777900a74a68](#q-777900a74a68d52773b2d9b912e306f466b2486dbb8e971701987edcce63a4e3), [Q-29721e95f740](#q-29721e95f74000bda38c5d50b713c3eaa1349f332368db89816c860cb1726cda) |
| FACT | 2024 | 2042 | 0 / 0 | 0 | 0 / 46 | 0 | 0 | [Q-833387c6a788](#q-833387c6a78802eabc66a6508a66fb766253d99c9778d343ee82e1c9c20dfc25), [Q-8120005ec4aa](#q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8), [Q-20549587a2d8](#q-20549587a2d82fd8f9264604f5d22265709b447ed75590d0c377e11165ff7b4a), [Q-5950de39204d](#q-5950de39204df7a9fc18604a10be1c579a11eed51296c89aaa0ee92391c77d83), [Q-6e2f84b370f7](#q-6e2f84b370f723c027a4c92634cedb4e282f895d9b91ff9e9e0d4abfc0c37192) |
| FACT | 2025 | 2351 | 0 / 0 | 0 | 1 / 92 | 0 | 0 | [Q-4162b4a6197d](#q-4162b4a6197dcb0b9c1421f5105fbe7f5f622d34563f017697d843f598049b22), [Q-6eb3a1b4eb6b](#q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8), [Q-38f4a602f2df](#q-38f4a602f2df6574276469fc3d519c9684171bd70dcefc1725b1ba4083453723), [Q-36c1a33b7900](#q-36c1a33b7900376bd306750eff4cd6dbde603da187381f99185d63961906f7f9), [Q-5e2981697017](#q-5e298169701732222fa28fc300ab125c3cecd505791fb565296c46dfb53c9572) |

FACT: 2015: main-window rows=1662; unclassified=1616; adjacent-month explicit events=0. [Q-d9c8e40ee22d](#q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b), [Q-233d80c8096f](#q-233d80c8096f1f7ef00061c24a09259889c2e2d115436ee935ca298126aee34c), [Q-19e72096bdc0](#q-19e72096bdc0bede16287aa14b1b1d6ac6dc1bd61adaeb6645bdde0e403b686d), [Q-43e9763d2d3f](#q-43e9763d2d3f89e3f3fbfe9b517cdadf2a577cc09afe70cdadb0f0a58d3dd8dd), [Q-5283c1735911](#q-5283c1735911819cc64fa9125120c494ac86befd74fdd54fe39542932b0f1ecc)

FACT: 2015 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 587 |
| FACT | SC | Status Change | 335 |
| FACT | SFA | Signed as Free Agent | 326 |
| FACT | TR | Trade | 122 |
| FACT | DFA | Declared Free Agency | 46 |
| FACT | DES | Designated for Assignment | 27 |
| FACT | REL | Released | 95 |
| FACT | CLW | Claimed Off Waivers | 91 |
| FACT | OUT | Outrighted | 10 |
| FACT | RET | Retired | 1 |
| FACT | LON | Loan | 1 |
| FACT | SGN | Signed | 1 |
| FACT | NUM | Number Change | 5 |
| FACT | CP | Contract Purchased | 1 |
| FACT | TRN | Transferred | 14 |

FACT: Example 2015-12-01, person ID=501647, code=DFA, from=unavailable, to=342: 1B Wilin Rosario elected free agency. [Q-d9c8e40ee22d](#q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2015-12-01, person ID=502226, code=DFA, from=unavailable, to=556: LF Craig Gentry elected free agency. [Q-d9c8e40ee22d](#q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2015-12-02, person ID=605333, code=DFA, from=unavailable, to=112: LHP Jack Leathersich elected free agency. [Q-d9c8e40ee22d](#q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2015: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2016: main-window rows=1378; unclassified=1340; adjacent-month explicit events=0. [Q-0a2b75bb513f](#q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3), [Q-c390ddd9fa88](#q-c390ddd9fa8850b2b4f5169fa77734af1858d07ba2f31be2e05a8026974fe5dd), [Q-3d232100e262](#q-3d232100e262f518c01cba3ce51f8b50363b66d801d582141396b1f9f57bfc06), [Q-f4871493ed80](#q-f4871493ed80a598a5cb41672d1f73f4565bdb0ea257e74eb2035919afca37a2), [Q-c3d5e6f973e7](#q-c3d5e6f973e753ab956046f114b0de198208929d7e70f23ba2382ab081773653)

FACT: 2016 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 501 |
| FACT | SC | Status Change | 241 |
| FACT | SFA | Signed as Free Agent | 275 |
| FACT | OUT | Outrighted | 14 |
| FACT | TR | Trade | 74 |
| FACT | DES | Designated for Assignment | 21 |
| FACT | DFA | Declared Free Agency | 38 |
| FACT | CLW | Claimed Off Waivers | 72 |
| FACT | REL | Released | 115 |
| FACT | NUM | Number Change | 10 |
| FACT | RET | Retired | 5 |
| FACT | TRN | Transferred | 12 |

FACT: Example 2016-12-02, person ID=643297, code=DFA, from=unavailable, to=108: LHP Cody Ege elected free agency. [Q-0a2b75bb513f](#q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2016-12-02, person ID=457754, code=DFA, from=unavailable, to=135: RHP Jon Edwards elected free agency. [Q-0a2b75bb513f](#q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2016-12-02, person ID=605304, code=DFA, from=unavailable, to=135: RHP Erik Johnson elected free agency. [Q-0a2b75bb513f](#q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2016: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2017: main-window rows=1311; unclassified=1285; adjacent-month explicit events=0. [Q-94ea0454e66f](#q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993), [Q-4e5dc646ff22](#q-4e5dc646ff22f2d66c270f49bcbb62a0f2d5efe14fde796499e7e9cd9683a16d), [Q-2e696bb0604b](#q-2e696bb0604b14d5cc0d616c041d88e808c6d3e422d8851dd31367745c42786a), [Q-14ac1c3b2718](#q-14ac1c3b2718a561b559acdb839a641418de2b10c6df2f8b626d8f1402105d8a), [Q-8814b82fd0b2](#q-8814b82fd0b2f67c21f783a9f6b27b6eec037788220f4ef6bd7efb984bb2acb9)

FACT: 2017 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 328 |
| FACT | DFA | Declared Free Agency | 26 |
| FACT | ASG | Assigned | 417 |
| FACT | SFA | Signed as Free Agent | 211 |
| FACT | REL | Released | 122 |
| FACT | TR | Trade | 100 |
| FACT | CLW | Claimed Off Waivers | 68 |
| FACT | DES | Designated for Assignment | 9 |
| FACT | NUM | Number Change | 6 |
| FACT | LON | Loan | 2 |
| FACT | OUT | Outrighted | 2 |
| FACT | RET | Retired | 7 |
| FACT | SGN | Signed | 1 |
| FACT | TRN | Transferred | 12 |

FACT: Example 2017-12-01, person ID=542454, code=DFA, from=unavailable, to=144: LF Danny Santana elected free agency. [Q-94ea0454e66f](#q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2017-12-01, person ID=444468, code=DFA, from=unavailable, to=112: RHP Hector Rondon elected free agency. [Q-94ea0454e66f](#q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2017-12-01, person ID=593643, code=DFA, from=unavailable, to=140: 3B Hanser Alberto elected free agency. [Q-94ea0454e66f](#q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2017: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2018: main-window rows=1253; unclassified=1252; adjacent-month explicit events=0. [Q-f879c3f65b13](#q-f879c3f65b13541057d672ed63a5b9f8b1801322341a2761dbe21060a61ff3fe), [Q-4bbef26ec32f](#q-4bbef26ec32f48840f279b8326e3212c62d41766e3ef18fc936713b1a2103359), [Q-d8a617158995](#q-d8a6171589959c2cdd77e8cca8e42e05ae4c3d8809d5aa82ceb1a735e160ff88), [Q-70c75d3b5e7e](#q-70c75d3b5e7ed79b7c1b7230b70f8a9bac75ee759d7138adbf03efe251bb741c), [Q-afb5d1a97b50](#q-afb5d1a97b505004cbe28ab0cd10f607ee2fefa3d7db7bb8123336c6a9a7ab1b)

FACT: 2018 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 371 |
| FACT | SC | Status Change | 425 |
| FACT | TR | Trade | 86 |
| FACT | SFA | Signed as Free Agent | 201 |
| FACT | DES | Designated for Assignment | 9 |
| FACT | LON | Loan | 1 |
| FACT | REL | Released | 61 |
| FACT | OUT | Outrighted | 2 |
| FACT | CLW | Claimed Off Waivers | 63 |
| FACT | NUM | Number Change | 7 |
| FACT | DFA | Declared Free Agency | 1 |
| FACT | RET | Retired | 2 |
| FACT | SGN | Signed | 1 |
| FACT | TRN | Transferred | 23 |

FACT: Example 2018-12-18, person ID=676786, code=DFA, from=unavailable, to=551: LHP Brandon Presley elected free agency. [Q-d8a617158995](#q-d8a6171589959c2cdd77e8cca8e42e05ae4c3d8809d5aa82ceb1a735e160ff88)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2018: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2019: main-window rows=1531; unclassified=1474; adjacent-month explicit events=0. [Q-27d37c5e2f42](#q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a), [Q-e99ef1dee109](#q-e99ef1dee1093a44b99bd7015739384610d246125a4be8a5596548cd4b8ba8f2), [Q-4b325fb28d97](#q-4b325fb28d97bd86f5e7f44cb9e5ee324f97a8f19129aaea2b8e371982da9719), [Q-c0d97ed051c2](#q-c0d97ed051c2352f9461be18c5d9b7abed7b1d3a54a3dbbcbdf685bfe060a925), [Q-4aebfb6dd7ec](#q-4aebfb6dd7ec76d8e957293bd8acb9acb2dbf206a13075f3beb6941442953ae7)

FACT: 2019 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 430 |
| FACT | ASG | Assigned | 475 |
| FACT | SFA | Signed as Free Agent | 311 |
| FACT | DFA | Declared Free Agency | 57 |
| FACT | RET | Retired | 8 |
| FACT | TR | Trade | 71 |
| FACT | DES | Designated for Assignment | 18 |
| FACT | CLW | Claimed Off Waivers | 67 |
| FACT | NUM | Number Change | 9 |
| FACT | REL | Released | 59 |
| FACT | LON | Loan | 8 |
| FACT | SGN | Signed | 12 |
| FACT | SE | Selected | 1 |
| FACT | OUT | Outrighted | 5 |

FACT: Example 2019-12-02, person ID=657610, code=DFA, from=unavailable, to=142: RHP Trevor Hildenberger elected free agency. [Q-27d37c5e2f42](#q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2019-12-02, person ID=607345, code=DFA, from=unavailable, to=108: C Kevan Smith elected free agency. [Q-27d37c5e2f42](#q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2019-12-02, person ID=457915, code=DFA, from=unavailable, to=120: RHP Javy Guerra elected free agency. [Q-27d37c5e2f42](#q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2019: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2020: main-window rows=2578; unclassified=2520; adjacent-month explicit events=0. [Q-2c5d9ad70bdc](#q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70), [Q-e04a4e8d49ce](#q-e04a4e8d49ce851fa6c27ff10057930cd64f6d0aa2c291ed695f1a55f6e2bbda), [Q-01ecfec45634](#q-01ecfec456342435b221f8f68f7ffa850a3ab1fefe9113d30e97314b411c6977), [Q-805d274a19a9](#q-805d274a19a9f49fe09b06fe9188983bc8aa7d9486295a7fa0d3454e1de73515), [Q-fd397dc3e730](#q-fd397dc3e730ee27a4a1d12d9ae6ea437095113c36949ef432876d50f1700567)

FACT: 2020 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 997 |
| FACT | SC | Status Change | 902 |
| FACT | SFA | Signed as Free Agent | 315 |
| FACT | DES | Designated for Assignment | 7 |
| FACT | LON | Loan | 15 |
| FACT | NUM | Number Change | 47 |
| FACT | TR | Trade | 55 |
| FACT | SE | Selected | 1 |
| FACT | CLW | Claimed Off Waivers | 132 |
| FACT | DFA | Declared Free Agency | 58 |
| FACT | OUT | Outrighted | 4 |
| FACT | REL | Released | 41 |
| FACT | CU | Recalled | 1 |
| FACT | RET | Retired | 2 |
| FACT | DEC | Deceased | 1 |

FACT: Example 2020-12-02, person ID=592325, code=DFA, from=unavailable, to=158: LF Ben Gamel elected free agency. [Q-2c5d9ad70bdc](#q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2020-12-02, person ID=607074, code=DFA, from=unavailable, to=145: LHP Carlos Rodon elected free agency. [Q-2c5d9ad70bdc](#q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2020-12-02, person ID=607054, code=DFA, from=unavailable, to=158: 3B Jace Peterson elected free agency. [Q-2c5d9ad70bdc](#q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2020: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2021: main-window rows=1780; unclassified=1778; adjacent-month explicit events=0. [Q-e4d3ae0895ed](#q-e4d3ae0895edecb34cef8a6548bb80e7b7e8863cea9301c052fe2a67c4223bc4), [Q-0e4cd9ba2efc](#q-0e4cd9ba2efcf3ba0699665547a13e0d719fec4944e69b2b95609a228930366d), [Q-8a05cb012f01](#q-8a05cb012f015939fc8e42057023e43a057e665cebe3817f2870ee439f6c7d64), [Q-ba0156e53a9a](#q-ba0156e53a9a30df2ed1e2f1350193915f08de5e7de4b9225353ce50bb160464), [Q-cc532385b8b5](#q-cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52)

FACT: 2021 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1150 |
| FACT | SFA | Signed as Free Agent | 199 |
| FACT | ASG | Assigned | 264 |
| FACT | NUM | Number Change | 10 |
| FACT | DES | Designated for Assignment | 3 |
| FACT | TR | Trade | 27 |
| FACT | OUT | Outrighted | 2 |
| FACT | LON | Loan | 2 |
| FACT | CLW | Claimed Off Waivers | 51 |
| FACT | REL | Released | 66 |
| FACT | RET | Retired | 4 |
| FACT | DFA | Declared Free Agency | 2 |

FACT: Example 2021-12-31, person ID=641772, code=DFA, from=unavailable, to=5508: RHP Joe Kuzia elected free agency. [Q-cc532385b8b5](#q-cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2021-12-31, person ID=554239, code=DFA, from=unavailable, to=525: SS Elmer Reyes elected free agency. [Q-cc532385b8b5](#q-cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2021: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2022: main-window rows=2745; unclassified=2739; adjacent-month explicit events=0. [Q-2fbb208a9d37](#q-2fbb208a9d374dc3045e6bb120b700c36d43cd10d5c71caa9f4fa1c80b9a0222), [Q-01b69e34c554](#q-01b69e34c5545e8a130038b64f2cdf4a185e8e44f0d9bef4c5b29f1f14ea6eb0), [Q-18f23a4cf0b6](#q-18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b), [Q-2a9b6f78ee40](#q-2a9b6f78ee4062ba791fd3dce3a1bf5a22f39217c729712572afd2971321f86c), [Q-6f26709b0806](#q-6f26709b0806358cb39c2a8964aec6bc14c22ec6b573d071387b483af9d84a81)

FACT: 2022 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | ASG | Assigned | 636 |
| FACT | SC | Status Change | 1488 |
| FACT | SFA | Signed as Free Agent | 333 |
| FACT | OUT | Outrighted | 15 |
| FACT | CLW | Claimed Off Waivers | 96 |
| FACT | TR | Trade | 51 |
| FACT | REL | Released | 49 |
| FACT | NUM | Number Change | 2 |
| FACT | DES | Designated for Assignment | 44 |
| FACT | RET | Retired | 8 |
| FACT | RTN | Returned | 2 |
| FACT | LON | Loan | 5 |
| FACT | DFA | Declared Free Agency | 6 |
| FACT | DR | Drafted | 10 |

FACT: Example 2022-12-18, person ID=658069, code=DFA, from=unavailable, to=496: 3B Joshua Fuentes elected free agency. [Q-18f23a4cf0b6](#q-18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2022-12-20, person ID=605353, code=DFA, from=unavailable, to=400: 3B Vimael Machín elected free agency. [Q-18f23a4cf0b6](#q-18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2022-12-23, person ID=605463, code=DFA, from=unavailable, to=1410: RHP Tayler Scott elected free agency. [Q-2a9b6f78ee40](#q-2a9b6f78ee4062ba791fd3dce3a1bf5a22f39217c729712572afd2971321f86c)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2022: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2023: main-window rows=2002; unclassified=2000; adjacent-month explicit events=0. [Q-c2b708962c1a](#q-c2b708962c1aa44d3a7cce177a83cc6c2488ed63a32347ebcd5865a42ed813a3), [Q-24af1ee31dcf](#q-24af1ee31dcfa11e4d414e44b7c2464693e9e3182528045ba4f62c6773a1af5b), [Q-dca79c9efd5c](#q-dca79c9efd5c5232559662e41bed8fe0f77a7c152a464c17e17c71bd630ab38a), [Q-777900a74a68](#q-777900a74a68d52773b2d9b912e306f466b2486dbb8e971701987edcce63a4e3), [Q-29721e95f740](#q-29721e95f74000bda38c5d50b713c3eaa1349f332368db89816c860cb1726cda)

FACT: 2023 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 927 |
| FACT | ASG | Assigned | 595 |
| FACT | SFA | Signed as Free Agent | 243 |
| FACT | REL | Released | 49 |
| FACT | DFA | Declared Free Agency | 2 |
| FACT | OUT | Outrighted | 5 |
| FACT | CLW | Claimed Off Waivers | 11 |
| FACT | TR | Trade | 73 |
| FACT | DR | Drafted | 78 |
| FACT | SE | Selected | 2 |
| FACT | DES | Designated for Assignment | 14 |
| FACT | RET | Retired | 1 |
| FACT | NUM | Number Change | 2 |

FACT: Example 2023-12-01, person ID=688024, code=DFA, from=unavailable, to=525: LHP Danny Wirchansky elected free agency. [Q-c2b708962c1a](#q-c2b708962c1aa44d3a7cce177a83cc6c2488ed63a32347ebcd5865a42ed813a3)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2023-12-10, person ID=656514, code=DFA, from=unavailable, to=145: CF Adam Haseley elected free agency. [Q-24af1ee31dcf](#q-24af1ee31dcfa11e4d414e44b7c2464693e9e3182528045ba4f62c6773a1af5b)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: 2023: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2024: main-window rows=2042; unclassified=1960; adjacent-month explicit events=0. [Q-833387c6a788](#q-833387c6a78802eabc66a6508a66fb766253d99c9778d343ee82e1c9c20dfc25), [Q-8120005ec4aa](#q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8), [Q-20549587a2d8](#q-20549587a2d82fd8f9264604f5d22265709b447ed75590d0c377e11165ff7b4a), [Q-5950de39204d](#q-5950de39204df7a9fc18604a10be1c579a11eed51296c89aaa0ee92391c77d83), [Q-6e2f84b370f7](#q-6e2f84b370f723c027a4c92634cedb4e282f895d9b91ff9e9e0d4abfc0c37192)

FACT: 2024 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SC | Status Change | 1034 |
| FACT | ASG | Assigned | 576 |
| FACT | RTN | Returned | 3 |
| FACT | NUM | Number Change | 12 |
| FACT | SFA | Signed as Free Agent | 218 |
| FACT | TR | Trade | 70 |
| FACT | OUT | Outrighted | 2 |
| FACT | REL | Released | 25 |
| FACT | DES | Designated for Assignment | 15 |
| FACT | R5M | Rule 5 Draft Minors | 67 |
| FACT | R5 | Rule 5 Selection | 15 |
| FACT | LON | Loan | 2 |
| FACT | RET | Retired | 1 |
| FACT | CLW | Claimed Off Waivers | 2 |

FACT: Example 2024-12-11, person ID=691634, code=R5M, from=507, to=494: Chicago White Sox purchased contract of RHP Joseph Yabbour from New York Mets in the Rule 5 Draft, AAA Phase. [Q-8120005ec4aa](#q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2024-12-11, person ID=666768, code=R5, from=146, to=144: Atlanta Braves purchased contract of RHP Anderson Pilar from Miami Marlins in the Rule 5 Draft. [Q-8120005ec4aa](#q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2024-12-11, person ID=691711, code=R5M, from=453, to=235: St. Louis Cardinals purchased contract of RHP Jawilme Ramirez from New York Mets in the Rule 5 Draft, AAA Phase. [Q-8120005ec4aa](#q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: 2024: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

FACT: 2025: main-window rows=2350; unclassified=2281; adjacent-month explicit events=0. [Q-4162b4a6197d](#q-4162b4a6197dcb0b9c1421f5105fbe7f5f622d34563f017697d843f598049b22), [Q-6eb3a1b4eb6b](#q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8), [Q-38f4a602f2df](#q-38f4a602f2df6574276469fc3d519c9684171bd70dcefc1725b1ba4083453723), [Q-36c1a33b7900](#q-36c1a33b7900376bd306750eff4cd6dbde603da187381f99185d63961906f7f9), [Q-5e2981697017](#q-5e298169701732222fa28fc300ab125c3cecd505791fb565296c46dfb53c9572)

FACT: 2025 observed type inventory (codes are not classification rules).

| Label | Code | Description | Observed rows |
|---|---|---|---|
| FACT | SFA | Signed as Free Agent | 291 |
| FACT | REL | Released | 54 |
| FACT | ASG | Assigned | 601 |
| FACT | SC | Status Change | 1141 |
| FACT | TR | Trade | 90 |
| FACT | LON | Loan | 19 |
| FACT | DES | Designated for Assignment | 28 |
| FACT | CLW | Claimed Off Waivers | 12 |
| FACT | OUT | Outrighted | 10 |
| FACT | OPT | Optioned | 1 |
| FACT | DFA | Declared Free Agency | 1 |
| FACT | NUM | Number Change | 29 |
| FACT | RET | Retired | 5 |
| FACT | R5 | Rule 5 Selection | 13 |
| FACT | R5M | Rule 5 Draft Minors | 55 |

FACT: Example 2025-12-07, person ID=682653, code=DFA, from=unavailable, to=342: 3B Warming Bernabel elected free agency. [Q-4162b4a6197d](#q-4162b4a6197dcb0b9c1421f5105fbe7f5f622d34563f017697d843f598049b22)

INFERENCE: Text-rule classification: `free_agency_ambiguous`.

FACT: Example 2025-12-10, person ID=685112, code=R5, from=146, to=143: Philadelphia Phillies purchased contract of RHP Zach McCambley  in the Rule 5 Draft. [Q-6eb3a1b4eb6b](#q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2025-12-10, person ID=696521, code=R5M, from=158, to=142: Minnesota Twins purchased contract of RF Garrett Spain  in the Rule 5 Draft, AAA Phase. [Q-6eb3a1b4eb6b](#q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: Example 2025-12-10, person ID=689450, code=R5M, from=115, to=116: Detroit Tigers purchased contract of RHP Luke Taggart  in the Rule 5 Draft, AAA Phase. [Q-6eb3a1b4eb6b](#q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8)

INFERENCE: Text-rule classification: `rule5_other`.

FACT: 2025: unresolved people joins=0/0; minor-stat group/level responses empty=0/0, failed=0/0, conflicting=0/0. A zero denominator means no identified players were queried. Per-player sources follow; failed operations are in diagnostics.

UNKNOWN: No observed selection does not establish that no draft occurred. No independent official draft list was retrieved in this implementation; ambiguous phase records require corroboration before a complete cohort can be claimed. A missing original/selecting club remains unresolved.

INFERENCE: `fromTeam.id` and `toTeam.id` on an explicit selection are candidate original and selecting club keys. Later trade records remain separate. Return confirmation requires explicit Rule 5 return text and matching reversed clubs; trades or missing clubs leave return evidence ambiguous. No return observed through the cutoff does not mean retained.

INFERENCE: Prior-season statistics could be draft-time inputs; selection and later return records are hindsight labels. Current biographies do not prove the identity attributes available on the draft date.

## C. CALLUP

INFERENCE: **PARTIAL (provisional)**: retrospective game-date filtering is testable, but strict as-published decision-time reconstruction is unproven. Sample success cannot establish population-wide coverage.

FACT: The configured bound is 56 player-level cases: one hitter and one pitcher for each of four levels and seven seasons. The historical API season-stat query requests the first games-played leader per group/level; cached ordering fixes replay, but server tie-breaking is unverified. No later MLB participation is used to select players.

| Label | Year | AAA completed / 2 | AA completed / 2 | A+ completed / 2 | A completed / 2 |
|---|---|---|---|---|---|
| FACT | 2018 | 2 | 2 | 2 | 2 |
| FACT | 2019 | 1 | 2 | 2 | 2 |
| FACT | 2021 | 2 | 2 | 2 | 2 |
| FACT | 2022 | 1 | 2 | 2 | 2 |
| FACT | 2023 | 2 | 2 | 2 | 2 |
| FACT | 2024 | 1 | 2 | 2 | 2 |
| FACT | 2025 | 2 | 2 | 2 | 2 |

FACT: 2018 AAA hitting, person 658069: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-d6685381152c](#q-d6685381152c5707ef63afb7d31dabfe00e2a438ceae3bf2797fce3ca6e96902), [Q-67237f081038](#q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e)

FACT: 2018 AAA hitting log/season additive agreement: `{}`. [Q-67237f081038](#q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e), [Q-5d7a42d66eba](#q-5d7a42d66ebac487c49d8c2a5b494685cc9b2a366f7fc50f959e9f732acfa778)

FACT: 2018 AAA hitting, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "a5cde12acb991ebd49616937f1526423ca1cab2e34422582cbb7b7b6451ca40a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-67237f081038](#q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e), [Q-a5cde12acb99](#q-a5cde12acb991ebd49616937f1526423ca1cab2e34422582cbb7b7b6451ca40a)

FACT: 2018 AAA hitting, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "0882d9c4e6456409491b6127cd4cbae28f045c9aa583fbcf5a613258ad41e213", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-67237f081038](#q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e), [Q-0882d9c4e645](#q-0882d9c4e6456409491b6127cd4cbae28f045c9aa583fbcf5a613258ad41e213)

FACT: 2018 AAA hitting, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "1de6ff58fbc755f6437515fb743b80bc7ecf9cb74b172e2e949bc4800c682fee", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-67237f081038](#q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e), [Q-1de6ff58fbc7](#q-1de6ff58fbc755f6437515fb743b80bc7ecf9cb74b172e2e949bc4800c682fee)

FACT: 2018 AAA pitching, person 474039: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-7934e67dcaf5](#q-7934e67dcaf516ef644ade8a547f9a6a2f4e4451648ca08a041b646effa75824), [Q-2020931453c9](#q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0)

FACT: 2018 AAA pitching log/season additive agreement: `{}`. [Q-2020931453c9](#q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0), [Q-3f8d3c0b7ed0](#q-3f8d3c0b7ed03ea57e76eb65f821844f3ffe877ed4f222424b3f69ef1b6aa4f7)

FACT: 2018 AAA pitching, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "d2303de748fbf9f02a86f52e58d781f37a127865ec4420b447d2a70c72dd2d23", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-2020931453c9](#q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0), [Q-d2303de748fb](#q-d2303de748fbf9f02a86f52e58d781f37a127865ec4420b447d2a70c72dd2d23)

FACT: 2018 AAA pitching, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "3cafdb0480e9a5a62d08c20198bc7aa54641c3354c3c293c98601bd67870ef7b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-2020931453c9](#q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0), [Q-3cafdb0480e9](#q-3cafdb0480e9a5a62d08c20198bc7aa54641c3354c3c293c98601bd67870ef7b)

FACT: 2018 AAA pitching, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "da5c3181a9669d894844cc8e11e1e5d75249936b0423de6193c63f43cd2091cc", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-2020931453c9](#q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0), [Q-da5c3181a966](#q-da5c3181a9669d894844cc8e11e1e5d75249936b0423de6193c63f43cd2091cc)

FACT: 2018 AA hitting, person 656509: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-581398926def](#q-581398926defa230a4f86546485d7916fcebaeb25ef362099666b6f3615111fd), [Q-d57489e11662](#q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0)

FACT: 2018 AA hitting log/season additive agreement: `{}`. [Q-d57489e11662](#q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0), [Q-5c94e9b959e4](#q-5c94e9b959e42a967b5d10f1a9cefcf2bc916b36c1689f72570e87b700042e65)

FACT: 2018 AA hitting, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "d0d465f2e98873f2037e4be4449df58f743d964bd9780334c9b995c8dc472f10", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d57489e11662](#q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0), [Q-d0d465f2e988](#q-d0d465f2e98873f2037e4be4449df58f743d964bd9780334c9b995c8dc472f10)

FACT: 2018 AA hitting, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "c35ec0b3f822de91395a6063fb35d4c91dba8f23485f6bc810330fbeea761cf8", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d57489e11662](#q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0), [Q-c35ec0b3f822](#q-c35ec0b3f822de91395a6063fb35d4c91dba8f23485f6bc810330fbeea761cf8)

FACT: 2018 AA hitting, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "adfb9653b818702a1eb373f2779fd7576f584d1c1e00243cbd93a0acb675ba09", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d57489e11662](#q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0), [Q-adfb9653b818](#q-adfb9653b818702a1eb373f2779fd7576f584d1c1e00243cbd93a0acb675ba09)

FACT: 2018 AA pitching, person 641980: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-0c60fd1ce45e](#q-0c60fd1ce45ea001d1179d3523359456f4b979bb8f8bf5bcb0735f41259e442c), [Q-50806df9dc86](#q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f)

FACT: 2018 AA pitching log/season additive agreement: `{}`. [Q-50806df9dc86](#q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f), [Q-945ed6765c9f](#q-945ed6765c9f72f87b1c2cb55419ddfb0d517c956d95d6fac571ad7f2360ea34)

FACT: 2018 AA pitching, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "7ea60465bd00afeb99ff6c0c52208f9f7a870cbeaa1929dae8fbe6cee8319591", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-50806df9dc86](#q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f), [Q-7ea60465bd00](#q-7ea60465bd00afeb99ff6c0c52208f9f7a870cbeaa1929dae8fbe6cee8319591)

FACT: 2018 AA pitching, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "19df4c90fefe44c756ddec329111f546cf608cc58f8dafcdbc9ce3310b5baa7c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-50806df9dc86](#q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f), [Q-19df4c90fefe](#q-19df4c90fefe44c756ddec329111f546cf608cc58f8dafcdbc9ce3310b5baa7c)

FACT: 2018 AA pitching, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "a3bcd1c95efe59b68d920f96aa0d6e23b97fa7cfafacf04eb2b4bd5e4e4860e9", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-50806df9dc86](#q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f), [Q-a3bcd1c95efe](#q-a3bcd1c95efe59b68d920f96aa0d6e23b97fa7cfafacf04eb2b4bd5e4e4860e9)

FACT: 2018 A+ hitting, person 676631: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-560c8f9b8fff](#q-560c8f9b8fffd4b2d5f7ed57c036939ec868e5c6a8b51e217c7b38952e2ca00d), [Q-fb194b4f299d](#q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367)

FACT: 2018 A+ hitting log/season additive agreement: `{}`. [Q-fb194b4f299d](#q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367), [Q-c8a24f4c0edb](#q-c8a24f4c0edbc317e7e0b1614ddbc7cd916869cff76588c9b346dc218666c214)

FACT: 2018 A+ hitting, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "24216ebc77a1f829db9c064052058c60a56cd0448a1e9b96fa9942b3a2cb26e7", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-fb194b4f299d](#q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367), [Q-24216ebc77a1](#q-24216ebc77a1f829db9c064052058c60a56cd0448a1e9b96fa9942b3a2cb26e7)

FACT: 2018 A+ hitting, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "4c812c133059a0f756f02997789e5d0c833d7ebf8729771bb3d3d63b2be499aa", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-fb194b4f299d](#q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367), [Q-4c812c133059](#q-4c812c133059a0f756f02997789e5d0c833d7ebf8729771bb3d3d63b2be499aa)

FACT: 2018 A+ hitting, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f9dde909ddbc3a2812d087cfdd00ca4d65bdc10ba6d873db494cf6567000eed7", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-fb194b4f299d](#q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367), [Q-f9dde909ddbc](#q-f9dde909ddbc3a2812d087cfdd00ca4d65bdc10ba6d873db494cf6567000eed7)

FACT: 2018 A+ pitching, person 664875: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-9ce0475643c6](#q-9ce0475643c6d326a1378423bd1827703fe6bf9f2b69beab40f2dc1f6f852ab7), [Q-d579d931485e](#q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f)

FACT: 2018 A+ pitching log/season additive agreement: `{}`. [Q-d579d931485e](#q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f), [Q-064c1d9360ac](#q-064c1d9360ac42130052eb754f99aac18574d08de906b000d4e5fc2d6b419861)

FACT: 2018 A+ pitching, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "caed2fbc55a13e230367908da416cb4a69a98acc5109dd110656d6cb20a875b3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d579d931485e](#q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f), [Q-caed2fbc55a1](#q-caed2fbc55a13e230367908da416cb4a69a98acc5109dd110656d6cb20a875b3)

FACT: 2018 A+ pitching, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "e91d1fffdd972215ba8d808746241131097000a8a4c3fc9f5a3722f2708f8e51", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d579d931485e](#q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f), [Q-e91d1fffdd97](#q-e91d1fffdd972215ba8d808746241131097000a8a4c3fc9f5a3722f2708f8e51)

FACT: 2018 A+ pitching, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "847e199d8aaef93f246d4ae9bd8c9ca4ed7499c1a2fae09b962c58540f138de5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d579d931485e](#q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f), [Q-847e199d8aae](#q-847e199d8aaef93f246d4ae9bd8c9ca4ed7499c1a2fae09b962c58540f138de5)

FACT: 2018 A hitting, person 650626: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-47b6403864a4](#q-47b6403864a453553f3a34f4ed8edcb2d1e4107a4e914586b5c5da5e56aceabb), [Q-025b6373402d](#q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7)

FACT: 2018 A hitting log/season additive agreement: `{}`. [Q-025b6373402d](#q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7), [Q-a65420fedb48](#q-a65420fedb48a76b3ef23f4da536ddaa636ae53f0ed8aa9084862293ea7007ce)

FACT: 2018 A hitting, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "9bf6e7d0a6e58f282b71c62ac5ef3a8bc5f1c5302372b12168acb3fce24ebd8b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-025b6373402d](#q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7), [Q-9bf6e7d0a6e5](#q-9bf6e7d0a6e58f282b71c62ac5ef3a8bc5f1c5302372b12168acb3fce24ebd8b)

FACT: 2018 A hitting, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "08ea9cdea83f299d3202cf50a46474de263dcc91dd5b2b4c74176361d752fceb", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-025b6373402d](#q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7), [Q-08ea9cdea83f](#q-08ea9cdea83f299d3202cf50a46474de263dcc91dd5b2b4c74176361d752fceb)

FACT: 2018 A hitting, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "990b17b8d0dc352cda25a7ad5c9ff863b6d7425a2ddcae2d80d4b59fbb17be11", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-025b6373402d](#q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7), [Q-990b17b8d0dc](#q-990b17b8d0dc352cda25a7ad5c9ff863b6d7425a2ddcae2d80d4b59fbb17be11)

FACT: 2018 A pitching, person 656382: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-e1a7f586e0bd](#q-e1a7f586e0bd72bc3537c17bf07786e0c89cca9613cec69f2596cc192d0dcf73), [Q-d12ae0a22d3f](#q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196)

FACT: 2018 A pitching log/season additive agreement: `{}`. [Q-d12ae0a22d3f](#q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196), [Q-eaf82a7b0516](#q-eaf82a7b051662ac916b7f3ac23338e13146719321b4ff064985158eaeff29e2)

FACT: 2018 A pitching, cutoff 2018-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "e27f26d86cb8c887cfe36c6eb7bae76651d1f4c3eeda4836ae60c5b89380b964", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d12ae0a22d3f](#q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196), [Q-e27f26d86cb8](#q-e27f26d86cb8c887cfe36c6eb7bae76651d1f4c3eeda4836ae60c5b89380b964)

FACT: 2018 A pitching, cutoff 2018-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "39c9d35f3b367c088ebe105effe12332a8dc2060c9e80de4a997d453158078b3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d12ae0a22d3f](#q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196), [Q-39c9d35f3b36](#q-39c9d35f3b367c088ebe105effe12332a8dc2060c9e80de4a997d453158078b3)

FACT: 2018 A pitching, cutoff 2018-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2018-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "01e01ced12b24015763725e7cdfc434a0cc549927714f8706649f4ac7fefda5e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d12ae0a22d3f](#q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196), [Q-01e01ced12b2](#q-01e01ced12b24015763725e7cdfc434a0cc549927714f8706649f4ac7fefda5e)

UNKNOWN: 2019 AAA hitting: incomplete. [Q-7a3be1224d1b](#q-7a3be1224d1b7c0f4a6da18cf49145e4f4f058cd2801033898bee20f0fb2caf4)

FACT: 2019 AAA pitching, person 662964: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-ab0a1cd85e5f](#q-ab0a1cd85e5f39212fa09d74004e6ef71e7082220892231860bf2a006b442c96), [Q-cbcb327b8e18](#q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada)

FACT: 2019 AAA pitching log/season additive agreement: `{}`. [Q-cbcb327b8e18](#q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada), [Q-a6cb4395ed55](#q-a6cb4395ed559593e9f0b23cffce7d7887764622f1335e9a0473d4642b72dcea)

FACT: 2019 AAA pitching, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dc6e9528dde89878ce3a07dd42cd41d1bc66bcca24eb28b321bb807d3aac3be3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cbcb327b8e18](#q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada), [Q-dc6e9528dde8](#q-dc6e9528dde89878ce3a07dd42cd41d1bc66bcca24eb28b321bb807d3aac3be3)

FACT: 2019 AAA pitching, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "710219887d4422be4c031d418c9dfa6ddce431ac5bd65ee0330bb4c5ef0c06bd", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cbcb327b8e18](#q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada), [Q-710219887d44](#q-710219887d4422be4c031d418c9dfa6ddce431ac5bd65ee0330bb4c5ef0c06bd)

FACT: 2019 AAA pitching, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "54a579b20cd1d399eb3e2ced80d8adf86bfec38df15446fea38f6064ad805bcb", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cbcb327b8e18](#q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada), [Q-54a579b20cd1](#q-54a579b20cd1d399eb3e2ced80d8adf86bfec38df15446fea38f6064ad805bcb)

FACT: 2019 AA hitting, person 663630: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-8dbc9bae91d6](#q-8dbc9bae91d6ec2f11be357abfc423cd098b5dd6981a256469fc4c73388e625c), [Q-961af1d0d72a](#q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1)

FACT: 2019 AA hitting log/season additive agreement: `{}`. [Q-961af1d0d72a](#q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1), [Q-0651a1e98903](#q-0651a1e98903510b69f02fa732280b51d0679ac3f1073daad353be92a7661ae3)

FACT: 2019 AA hitting, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "6c5419795a8a71d88468c45c68fc0e014f6d947f90d5160f8bd70cf381205233", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-961af1d0d72a](#q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1), [Q-6c5419795a8a](#q-6c5419795a8a71d88468c45c68fc0e014f6d947f90d5160f8bd70cf381205233)

FACT: 2019 AA hitting, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "35de6b176d4ae37717ade5d86fbb9747a3a00a5ee63b33628593c3624a73c38b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-961af1d0d72a](#q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1), [Q-35de6b176d4a](#q-35de6b176d4ae37717ade5d86fbb9747a3a00a5ee63b33628593c3624a73c38b)

FACT: 2019 AA hitting, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "dcc7c90595a18bfb85e8b82b24e7ce36661741623ecdb3bbdf331d490bcb60e4", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-961af1d0d72a](#q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1), [Q-dcc7c90595a1](#q-dcc7c90595a18bfb85e8b82b24e7ce36661741623ecdb3bbdf331d490bcb60e4)

FACT: 2019 AA pitching, person 676754: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-393421dc4d2a](#q-393421dc4d2a8c82c3eaa83dcb4d7354ecdaafa093652bb3592ca22758e009b0), [Q-0b4341ae0cfc](#q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205)

FACT: 2019 AA pitching log/season additive agreement: `{}`. [Q-0b4341ae0cfc](#q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205), [Q-f1b0b1131afa](#q-f1b0b1131afa443e51f04c53945c42f9a84b8498e1ac4621f4bb8ad413f30ebb)

FACT: 2019 AA pitching, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "5c373e62a4a2e544b9caf7f969e6d5d335802d137bf83e4a648df1499ce3bc89", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0b4341ae0cfc](#q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205), [Q-5c373e62a4a2](#q-5c373e62a4a2e544b9caf7f969e6d5d335802d137bf83e4a648df1499ce3bc89)

FACT: 2019 AA pitching, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "10efde1bcc0671f533172b2fe9836dee0a413eaa66e209148306e4bfa25593c5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0b4341ae0cfc](#q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205), [Q-10efde1bcc06](#q-10efde1bcc0671f533172b2fe9836dee0a413eaa66e209148306e4bfa25593c5)

FACT: 2019 AA pitching, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "4c69d5d20b26277b6f01345256be8ef3ce1b28d7ae86e638a5257cd71c4b7082", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0b4341ae0cfc](#q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205), [Q-4c69d5d20b26](#q-4c69d5d20b26277b6f01345256be8ef3ce1b28d7ae86e638a5257cd71c4b7082)

FACT: 2019 A+ hitting, person 677690: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-74d5d3a2ba25](#q-74d5d3a2ba25df5e9ea10b165aac7fcb6d543121e45c9bb4add1f6a85ae883b7), [Q-95b4ddeae5c8](#q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e)

FACT: 2019 A+ hitting log/season additive agreement: `{}`. [Q-95b4ddeae5c8](#q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e), [Q-9aafdeae8e85](#q-9aafdeae8e856f976cbbc8875fb0b716ca4ec697682b8c608621af6366ff17dd)

FACT: 2019 A+ hitting, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "84b8d49b0dee99e18ca17d21b6301e17cdccc1fa82bd976f9c701f55541e9fc0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-95b4ddeae5c8](#q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e), [Q-84b8d49b0dee](#q-84b8d49b0dee99e18ca17d21b6301e17cdccc1fa82bd976f9c701f55541e9fc0)

FACT: 2019 A+ hitting, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f2db68df06a8d81dd777f3417ceb5cf51f6e4d78d260858818f78a96ba156bb9", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-95b4ddeae5c8](#q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e), [Q-f2db68df06a8](#q-f2db68df06a8d81dd777f3417ceb5cf51f6e4d78d260858818f78a96ba156bb9)

FACT: 2019 A+ hitting, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "b774d3657cb87a700d2d618d24e7dc1c398e75f7b25cc914d18b8eb6a08386b6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-95b4ddeae5c8](#q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e), [Q-b774d3657cb8](#q-b774d3657cb87a700d2d618d24e7dc1c398e75f7b25cc914d18b8eb6a08386b6)

FACT: 2019 A+ pitching, person 670442: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-6c43784a9ee7](#q-6c43784a9ee799450777efef71b9a6ef67fe067161d43c6b45edd0b7cdb524d3), [Q-e3df5fba44ad](#q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1)

FACT: 2019 A+ pitching log/season additive agreement: `{}`. [Q-e3df5fba44ad](#q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1), [Q-c95a73ecccb5](#q-c95a73ecccb592e651fd4c9e77aca5d44811e649d25d82738cdbd99ba7f465e0)

FACT: 2019 A+ pitching, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "e117766f09a7b650a26b0f2fe5dc406de4c55aeacb8fa2ad9229fba612a447e1", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e3df5fba44ad](#q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1), [Q-e117766f09a7](#q-e117766f09a7b650a26b0f2fe5dc406de4c55aeacb8fa2ad9229fba612a447e1)

FACT: 2019 A+ pitching, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "6c5b5e8ad9f73fbd8eacaa38b750d0f13276c00da831f0cc478637c94b0d646e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e3df5fba44ad](#q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1), [Q-6c5b5e8ad9f7](#q-6c5b5e8ad9f73fbd8eacaa38b750d0f13276c00da831f0cc478637c94b0d646e)

FACT: 2019 A+ pitching, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "bd5a95850e345dab11ee395943abe479876d574db316cdc756d2b8c6fdbc670f", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e3df5fba44ad](#q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1), [Q-bd5a95850e34](#q-bd5a95850e345dab11ee395943abe479876d574db316cdc756d2b8c6fdbc670f)

FACT: 2019 A hitting, person 681807: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-8de868442d95](#q-8de868442d9592f43793d874b35a0a5c74021316275fdd3b7e95313300830953), [Q-a7d1f3ca02a1](#q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f)

FACT: 2019 A hitting log/season additive agreement: `{}`. [Q-a7d1f3ca02a1](#q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f), [Q-1ea770d6e876](#q-1ea770d6e87639c39c30f99d9d8b4efb0cd7ac3e3db5bb1a4947cc05f66ef211)

FACT: 2019 A hitting, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "5f953bfec3f554f3493e988e18fe754cb762e19ffa4ded9f962f040aee206b6c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-a7d1f3ca02a1](#q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f), [Q-5f953bfec3f5](#q-5f953bfec3f554f3493e988e18fe754cb762e19ffa4ded9f962f040aee206b6c)

FACT: 2019 A hitting, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "822f42d2e3dfe16a8e429d633f565af12108cdd2f21df10b18d2f7d7f49503bc", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-a7d1f3ca02a1](#q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f), [Q-822f42d2e3df](#q-822f42d2e3dfe16a8e429d633f565af12108cdd2f21df10b18d2f7d7f49503bc)

FACT: 2019 A hitting, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "ecee20b0ecf4387e673664daa1aff4faefbe1eadc94d7b714d20168a387947ae", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-a7d1f3ca02a1](#q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f), [Q-ecee20b0ecf4](#q-ecee20b0ecf4387e673664daa1aff4faefbe1eadc94d7b714d20168a387947ae)

FACT: 2019 A pitching, person 676571: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-7d5b8f5b799d](#q-7d5b8f5b799d8526ee673098b94dea3c1d000a9ca9ce468f711c7da8ab84933f), [Q-c42229e15b50](#q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223)

FACT: 2019 A pitching log/season additive agreement: `{}`. [Q-c42229e15b50](#q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223), [Q-526cc03f5af1](#q-526cc03f5af1f8c0140dd2e37e420284f246d82bb1ca13c2b9f631aa0a196c76)

FACT: 2019 A pitching, cutoff 2019-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "415d9ba3bbffd8c2fd631f1c54cdb2935223048911e15f015fa1eb24b3ff1292", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c42229e15b50](#q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223), [Q-415d9ba3bbff](#q-415d9ba3bbffd8c2fd631f1c54cdb2935223048911e15f015fa1eb24b3ff1292)

FACT: 2019 A pitching, cutoff 2019-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "d2e1a14b62ab3a1e10f8def706a75e263c1bda450b3ae989a43895925fa0ba72", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c42229e15b50](#q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223), [Q-d2e1a14b62ab](#q-d2e1a14b62ab3a1e10f8def706a75e263c1bda450b3ae989a43895925fa0ba72)

FACT: 2019 A pitching, cutoff 2019-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2019-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "258a055c7d6ddc0bff1718a2ccaa1dd40f0fe6fed13902b9ceadd921728a7466", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c42229e15b50](#q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223), [Q-258a055c7d6d](#q-258a055c7d6ddc0bff1718a2ccaa1dd40f0fe6fed13902b9ceadd921728a7466)

FACT: 2021 AAA hitting, person 669742: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-cdf39135f546](#q-cdf39135f546e07a8c4b931aa38e2c8bbf56863d18478927746d05d96474557b), [Q-2532063af6d0](#q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8)

FACT: 2021 AAA hitting log/season additive agreement: `{}`. [Q-2532063af6d0](#q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8), [Q-a226fa376e40](#q-a226fa376e40273ab2e84844a3ec8ed5eec03488249794d2cbb8da685fc91c0b)

FACT: 2021 AAA hitting, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "9a54c9c0a290a29570688dfcbd645c710482f382a29b26b815317cf4d65880f8", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-2532063af6d0](#q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8), [Q-9a54c9c0a290](#q-9a54c9c0a290a29570688dfcbd645c710482f382a29b26b815317cf4d65880f8)

FACT: 2021 AAA hitting, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "a4b90a32427f0cae0abfc5cc73d855756947344f093d0e2b3a99a6f13d81e71e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-2532063af6d0](#q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8), [Q-a4b90a32427f](#q-a4b90a32427f0cae0abfc5cc73d855756947344f093d0e2b3a99a6f13d81e71e)

FACT: 2021 AAA hitting, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f6cc9c9b2d8bdf8a7edbc6b222fc16b2f2455ab6fbfe860b1a7991d8784b840d", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-2532063af6d0](#q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8), [Q-f6cc9c9b2d8b](#q-f6cc9c9b2d8bdf8a7edbc6b222fc16b2f2455ab6fbfe860b1a7991d8784b840d)

FACT: 2021 AAA pitching, person 670426: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-406e4b77f3f3](#q-406e4b77f3f3b0d80aa3fa9f08896508f19dff8caf227a151b2c70e0f77da746), [Q-c1e00c445379](#q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d)

FACT: 2021 AAA pitching log/season additive agreement: `{}`. [Q-c1e00c445379](#q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d), [Q-671352b42afb](#q-671352b42afbaf5ee9470b4f2b8e67ff8c307c05b82b901aa929196458d54227)

FACT: 2021 AAA pitching, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "478e94c7a1bc3ffe2e86d7205edb50dfdeb6d52dd22d0dfcdc306085c2381cd7", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c1e00c445379](#q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d), [Q-478e94c7a1bc](#q-478e94c7a1bc3ffe2e86d7205edb50dfdeb6d52dd22d0dfcdc306085c2381cd7)

FACT: 2021 AAA pitching, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "3b31ed05974676f4c1a613da4023b4f5435471bec054bf2dbb795ebf8aa4a4b4", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c1e00c445379](#q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d), [Q-3b31ed059746](#q-3b31ed05974676f4c1a613da4023b4f5435471bec054bf2dbb795ebf8aa4a4b4)

FACT: 2021 AAA pitching, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "f1b9fed12525224a2c122cac00d9ef6f471de09338932626f8621edaf30289bd", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-c1e00c445379](#q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d), [Q-f1b9fed12525](#q-f1b9fed12525224a2c122cac00d9ef6f471de09338932626f8621edaf30289bd)

FACT: 2021 AA hitting, person 669722: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-2c8fc5d909c2](#q-2c8fc5d909c2da3fa627ff047cb21d8a33ec8118f045d1a1a40b8de5d97969ef), [Q-9bcc720db2fe](#q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac)

FACT: 2021 AA hitting log/season additive agreement: `{}`. [Q-9bcc720db2fe](#q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac), [Q-cc00710c2dd9](#q-cc00710c2dd99ec1cc66e97f09957df5b2ca5445160a3233cde3377fc8b6883e)

FACT: 2021 AA hitting, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "7c3d9c919d71b4e54422d714688adef9a65be3a694781681cbb20e1b25313ff0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-9bcc720db2fe](#q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac), [Q-7c3d9c919d71](#q-7c3d9c919d71b4e54422d714688adef9a65be3a694781681cbb20e1b25313ff0)

FACT: 2021 AA hitting, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "377e87ab65fb5a41cfe52bebe74b961cd51af0d6f61ae8f89f5e79f984f28859", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-9bcc720db2fe](#q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac), [Q-377e87ab65fb](#q-377e87ab65fb5a41cfe52bebe74b961cd51af0d6f61ae8f89f5e79f984f28859)

FACT: 2021 AA hitting, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "68a91646fbf0af346597c9d69b83511c8d99ecc3ee4a6df773b7c524221ceca9", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-9bcc720db2fe](#q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac), [Q-68a91646fbf0](#q-68a91646fbf0af346597c9d69b83511c8d99ecc3ee4a6df773b7c524221ceca9)

FACT: 2021 AA pitching, person 646243: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-022da2494bf9](#q-022da2494bf982451531f8f22238a0df02eecaab03ed54d66e5d2b71caa32dcc), [Q-319b9e835ec9](#q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9)

FACT: 2021 AA pitching log/season additive agreement: `{}`. [Q-319b9e835ec9](#q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9), [Q-d34da7243fa6](#q-d34da7243fa64c26868c1f7585e4602c996177b8ca6ca29934be5a7e4c83c4a4)

FACT: 2021 AA pitching, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "61b57f5cd5051dcd29104915dd2b628e8b8ea89a458117c6f796617033c05e80", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-319b9e835ec9](#q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9), [Q-61b57f5cd505](#q-61b57f5cd5051dcd29104915dd2b628e8b8ea89a458117c6f796617033c05e80)

FACT: 2021 AA pitching, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "67d59feda549b8743c53e9a36854a09629b848a1ea084e8e8056d476fdb876f2", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-319b9e835ec9](#q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9), [Q-67d59feda549](#q-67d59feda549b8743c53e9a36854a09629b848a1ea084e8e8056d476fdb876f2)

FACT: 2021 AA pitching, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "18bf5ae6e9cb1477fff6ae5aa16621d865e661dfd45aab6c3a8884160e8cab4c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-319b9e835ec9](#q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9), [Q-18bf5ae6e9cb](#q-18bf5ae6e9cb1477fff6ae5aa16621d865e661dfd45aab6c3a8884160e8cab4c)

FACT: 2021 A+ hitting, person 681624: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-b7b6b78822bd](#q-b7b6b78822bd1c45c0558455dc4c5a35b6e1ea02ec9727bf9a8d1feddcc238db), [Q-0045a0ad6f48](#q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1)

FACT: 2021 A+ hitting log/season additive agreement: `{}`. [Q-0045a0ad6f48](#q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1), [Q-b42d3de143fd](#q-b42d3de143fd519725d5b580f35a425d79df4a0ba533986d261f32070f5cc474)

FACT: 2021 A+ hitting, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "b6f8c8203ead8cdf1a01122d4644a20a94d82723616bfc6920da9e7f87941331", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-0045a0ad6f48](#q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1), [Q-b6f8c8203ead](#q-b6f8c8203ead8cdf1a01122d4644a20a94d82723616bfc6920da9e7f87941331)

FACT: 2021 A+ hitting, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "4220bfeeb6ee06918201f4e7ee78c2edc0ea283975caad740953fc7452f084d6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-0045a0ad6f48](#q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1), [Q-4220bfeeb6ee](#q-4220bfeeb6ee06918201f4e7ee78c2edc0ea283975caad740953fc7452f084d6)

FACT: 2021 A+ hitting, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "69d47fd2ebb67ac33c816b0a74c823ad69a45ae33e69a4f51187c13ff1966cfc", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-0045a0ad6f48](#q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1), [Q-69d47fd2ebb6](#q-69d47fd2ebb67ac33c816b0a74c823ad69a45ae33e69a4f51187c13ff1966cfc)

FACT: 2021 A+ pitching, person 682121: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-aaa6e073d5d8](#q-aaa6e073d5d8eaa735ed0f653948025d9345d648db14a87eed064699d06a6bd1), [Q-37bf061513b4](#q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8)

FACT: 2021 A+ pitching log/season additive agreement: `{}`. [Q-37bf061513b4](#q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8), [Q-51e59fe53513](#q-51e59fe535136601f7e1d2e475a8401783adfe521692f5311bf6ac2638502272)

FACT: 2021 A+ pitching, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dcbc249799d5824be78fa407ebb18777fd62781ec7a90ab41ee903044558e95e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-37bf061513b4](#q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8), [Q-dcbc249799d5](#q-dcbc249799d5824be78fa407ebb18777fd62781ec7a90ab41ee903044558e95e)

FACT: 2021 A+ pitching, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "ebe6346cf5584e3b9c2f1b7ad88e4d04f5fa4f21bdc3a4a948513a135085b727", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-37bf061513b4](#q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8), [Q-ebe6346cf558](#q-ebe6346cf5584e3b9c2f1b7ad88e4d04f5fa4f21bdc3a4a948513a135085b727)

FACT: 2021 A+ pitching, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "abb11eaae5cb06f9b8d78724051fa75a9273bc0520b07ae1cb7eedbe082e7930", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-37bf061513b4](#q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8), [Q-abb11eaae5cb](#q-abb11eaae5cb06f9b8d78724051fa75a9273bc0520b07ae1cb7eedbe082e7930)

FACT: 2021 A hitting, person 682868: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-14468c069f69](#q-14468c069f6978dbbed3c02024d2eebc87da15880b0cf7bdebeab41757aa2c48), [Q-3678f350e868](#q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5)

FACT: 2021 A hitting log/season additive agreement: `{}`. [Q-3678f350e868](#q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5), [Q-5d2867eefc2a](#q-5d2867eefc2a7ed353c31f1823ca947cc1abe7efd1eb0463b0a03fbf918163bd)

FACT: 2021 A hitting, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "5788d5d1a1b9147a62286999b42aa26376b48e84378d5fab61ee59b362911918", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3678f350e868](#q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5), [Q-5788d5d1a1b9](#q-5788d5d1a1b9147a62286999b42aa26376b48e84378d5fab61ee59b362911918)

FACT: 2021 A hitting, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "28b4198e0146460a8512c731bdb3f375db62cdfd3659e19d316150cfca643ccc", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3678f350e868](#q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5), [Q-28b4198e0146](#q-28b4198e0146460a8512c731bdb3f375db62cdfd3659e19d316150cfca643ccc)

FACT: 2021 A hitting, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "3bb103e2f0cfd2ecbd72351de35f5c1efbe7b97ac97eda4a572cf92e1fc1c411", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3678f350e868](#q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5), [Q-3bb103e2f0cf](#q-3bb103e2f0cfd2ecbd72351de35f5c1efbe7b97ac97eda4a572cf92e1fc1c411)

FACT: 2021 A pitching, person 675848: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-771ff2437fcd](#q-771ff2437fcd62766c5ed19c64ef7c6e48bd54d4a305c70ac56a237a94585dd3), [Q-67aa90d57e81](#q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2)

FACT: 2021 A pitching log/season additive agreement: `{}`. [Q-67aa90d57e81](#q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2), [Q-403466caab91](#q-403466caab91d1ffe566e5a5ad461a67ae01113095975a946b94c5b7133a54d8)

FACT: 2021 A pitching, cutoff 2021-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "5f7cba1e248d81ca700f56f4d407a5bf34695969195e64fcf31556a33226dc41", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-67aa90d57e81](#q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2), [Q-5f7cba1e248d](#q-5f7cba1e248d81ca700f56f4d407a5bf34695969195e64fcf31556a33226dc41)

FACT: 2021 A pitching, cutoff 2021-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "6ca0b3d3f81b9ff2306ea613f4b5da79d7de871aba2016f9500b63a0d42b5253", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-67aa90d57e81](#q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2), [Q-6ca0b3d3f81b](#q-6ca0b3d3f81b9ff2306ea613f4b5da79d7de871aba2016f9500b63a0d42b5253)

FACT: 2021 A pitching, cutoff 2021-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2021-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "e4ed93557d007c072f4f5ca632e05e1748f5c1dad4daae0d374c1ae4434b774d", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-67aa90d57e81](#q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2), [Q-e4ed93557d00](#q-e4ed93557d007c072f4f5ca632e05e1748f5c1dad4daae0d374c1ae4434b774d)

UNKNOWN: 2022 AAA hitting: incomplete. [Q-8273e8ce1e9b](#q-8273e8ce1e9beb438dbe0da8fb45efb8651d0f8c19b98a41ebed269cef0d1ec8)

FACT: 2022 AAA pitching, person 545346: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-abbb56bb32f8](#q-abbb56bb32f8f25530aae197ac6fd04dd90941b16274b2076841c556192278d9), [Q-a2689b712109](#q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8)

FACT: 2022 AAA pitching log/season additive agreement: `{}`. [Q-a2689b712109](#q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8), [Q-1656f71c605e](#q-1656f71c605ef51388895c8dcd78798ce066adaec816a7b831041e96cea98274)

FACT: 2022 AAA pitching, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "3506add38b9885503effc43e22a52a194da855a4a89ff4cc36937626e9d4fe30", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-a2689b712109](#q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8), [Q-3506add38b98](#q-3506add38b9885503effc43e22a52a194da855a4a89ff4cc36937626e9d4fe30)

FACT: 2022 AAA pitching, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "fee580187d68fa2169afdc4c363974d75def203109b706c67a01518ea612fca5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-a2689b712109](#q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8), [Q-fee580187d68](#q-fee580187d68fa2169afdc4c363974d75def203109b706c67a01518ea612fca5)

FACT: 2022 AAA pitching, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "770932cdf8629738f1452494447f2a360cfebdbf0ce26d3c1df76fe23b3eb77a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-a2689b712109](#q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8), [Q-770932cdf862](#q-770932cdf8629738f1452494447f2a360cfebdbf0ce26d3c1df76fe23b3eb77a)

FACT: 2022 AA hitting, person 681624: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-6bbd96da19c5](#q-6bbd96da19c5d056cf3bfb21b31e8e88f7a0a8a07441309292cf0c9e3481cc1b), [Q-3cb1d41cd2cb](#q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a)

FACT: 2022 AA hitting log/season additive agreement: `{}`. [Q-3cb1d41cd2cb](#q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a), [Q-2388ccb4b4c1](#q-2388ccb4b4c1e480c7d052d626601318e8779c40b2bddb2298e7ebce649c5815)

FACT: 2022 AA hitting, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "7148278dd7390f0e2f3e5759afe7c01fecdcd0d048252fc4f0d8c3a65741d763", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3cb1d41cd2cb](#q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a), [Q-7148278dd739](#q-7148278dd7390f0e2f3e5759afe7c01fecdcd0d048252fc4f0d8c3a65741d763)

FACT: 2022 AA hitting, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "da16dba2e740aa1f05bfd73a29f382a74e3958b1019359c8706db2df47c968c3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3cb1d41cd2cb](#q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a), [Q-da16dba2e740](#q-da16dba2e740aa1f05bfd73a29f382a74e3958b1019359c8706db2df47c968c3)

FACT: 2022 AA hitting, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "d92a1b1da7eec4c79c33043cf4548df5f8059c42701f6e9fd05ce7f183e59f19", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-3cb1d41cd2cb](#q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a), [Q-d92a1b1da7ee](#q-d92a1b1da7eec4c79c33043cf4548df5f8059c42701f6e9fd05ce7f183e59f19)

FACT: 2022 AA pitching, person 688427: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-e684cfc6a4af](#q-e684cfc6a4af65fa0aa79c913bda677990738b49f9e0e6b30d1256600b42b287), [Q-5ffc38ef51dd](#q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b)

FACT: 2022 AA pitching log/season additive agreement: `{}`. [Q-5ffc38ef51dd](#q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b), [Q-576bf4631da2](#q-576bf4631da27741a03fdf31e21ea886c77f5783b962533b978b4ecb696dcb14)

FACT: 2022 AA pitching, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "8420a0788b6789ec6729da7ae897fb8f14372f9c617c64ae9547144896fe6d0b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-5ffc38ef51dd](#q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b), [Q-8420a0788b67](#q-8420a0788b6789ec6729da7ae897fb8f14372f9c617c64ae9547144896fe6d0b)

FACT: 2022 AA pitching, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "07b093532b851023eab41205c8243ddfc0f91f019bcbb609116e3e066a0b3a5f", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-5ffc38ef51dd](#q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b), [Q-07b093532b85](#q-07b093532b851023eab41205c8243ddfc0f91f019bcbb609116e3e066a0b3a5f)

FACT: 2022 AA pitching, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "828088ba669affc85a569ec96f6d122330c44bd424a00e02d67c7cfc97375133", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-5ffc38ef51dd](#q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b), [Q-828088ba669a](#q-828088ba669affc85a569ec96f6d122330c44bd424a00e02d67c7cfc97375133)

FACT: 2022 A+ hitting, person 678391: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-1ddf27343009](#q-1ddf27343009089aa00bad50de553d4aac5fab52fd0cfc8806d75b6bbdea5763), [Q-630493a50098](#q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83)

FACT: 2022 A+ hitting log/season additive agreement: `{}`. [Q-630493a50098](#q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83), [Q-0ff9fbbac1fb](#q-0ff9fbbac1fb1006edf19d15c7ff9ac36aac22422d420cd4f2e105e7cf48acf9)

FACT: 2022 A+ hitting, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "0f3bc5a534542b8067f687cd06f8a38b934d96f7ab1c45d6225a6e0082a282c6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-630493a50098](#q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83), [Q-0f3bc5a53454](#q-0f3bc5a534542b8067f687cd06f8a38b934d96f7ab1c45d6225a6e0082a282c6)

FACT: 2022 A+ hitting, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "aaf48bde1efb8542167b657220a6951a7c67a31a859e9c5fb813c38d52059756", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-630493a50098](#q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83), [Q-aaf48bde1efb](#q-aaf48bde1efb8542167b657220a6951a7c67a31a859e9c5fb813c38d52059756)

FACT: 2022 A+ hitting, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "86b550ef80efc8f3331564e7a640949ab127d120955dabaddf4706794eff63e5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-630493a50098](#q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83), [Q-86b550ef80ef](#q-86b550ef80efc8f3331564e7a640949ab127d120955dabaddf4706794eff63e5)

FACT: 2022 A+ pitching, person 687841: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-6a525bcc25ab](#q-6a525bcc25ab979a2e4a00e39dd1f293bbd9afc0c2c9adf34d902398406dde9f), [Q-e87402d7a1c3](#q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55)

FACT: 2022 A+ pitching log/season additive agreement: `{}`. [Q-e87402d7a1c3](#q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55), [Q-7f2e72894aa9](#q-7f2e72894aa970ec8f8bc38b70568c35518f91dbc77ae8ecbfd3bc3d65971ea6)

FACT: 2022 A+ pitching, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "f94ba1a1004e07606ea3d035bb968699ed7cefbb2e5d960a5651b342e26d27c0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e87402d7a1c3](#q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55), [Q-f94ba1a1004e](#q-f94ba1a1004e07606ea3d035bb968699ed7cefbb2e5d960a5651b342e26d27c0)

FACT: 2022 A+ pitching, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "826949136b9b06d2bf26c3626ec15c6be5ed1bea83142892da1af852e0ad47e9", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e87402d7a1c3](#q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55), [Q-826949136b9b](#q-826949136b9b06d2bf26c3626ec15c6be5ed1bea83142892da1af852e0ad47e9)

FACT: 2022 A+ pitching, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "9996380e6659677c675b10a37632890a77dcaf9db364dab20d174e50d84f9163", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e87402d7a1c3](#q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55), [Q-9996380e6659](#q-9996380e6659677c675b10a37632890a77dcaf9db364dab20d174e50d84f9163)

FACT: 2022 A hitting, person 692348: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-e34ca5abe565](#q-e34ca5abe565cce598398f66310b002cf9104e9dca3bed902427e18a040880c2), [Q-5da751e70d49](#q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa)

FACT: 2022 A hitting log/season additive agreement: `{}`. [Q-5da751e70d49](#q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa), [Q-9fa15951a758](#q-9fa15951a75874e3d70b741d452fa2a2d1e3993fe6a57abb6bd906421c587126)

FACT: 2022 A hitting, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "1895eecd023896174d0c8424ca860c5eed1d3fd9c9e5bc365b486a25f203cad5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5da751e70d49](#q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa), [Q-1895eecd0238](#q-1895eecd023896174d0c8424ca860c5eed1d3fd9c9e5bc365b486a25f203cad5)

FACT: 2022 A hitting, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f35524e664842e9d0060bdc4816dcce943b5d9cdbe112c3f6d39f153a84784f8", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5da751e70d49](#q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa), [Q-f35524e66484](#q-f35524e664842e9d0060bdc4816dcce943b5d9cdbe112c3f6d39f153a84784f8)

FACT: 2022 A hitting, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "1e7ceb892dc2ebe528e4fc057774d4a3294f55d2ba44e00f07d775727b1e09b6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5da751e70d49](#q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa), [Q-1e7ceb892dc2](#q-1e7ceb892dc2ebe528e4fc057774d4a3294f55d2ba44e00f07d775727b1e09b6)

FACT: 2022 A pitching, person 683928: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-c7b74974eced](#q-c7b74974eced82d0432d25e983daaf58c20aa552cf66b38137da09874beca85e), [Q-e9b5c019c9da](#q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a)

FACT: 2022 A pitching log/season additive agreement: `{}`. [Q-e9b5c019c9da](#q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a), [Q-3e2a54f3f77f](#q-3e2a54f3f77fb4e55ea07aa0de9c5853c1b1ff07e54071479592b233efcd9b21)

FACT: 2022 A pitching, cutoff 2022-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "3d53415bc6bd566f8725c41bc1d254ec163cdc021ce675592e7cd60ed395ac56", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e9b5c019c9da](#q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a), [Q-3d53415bc6bd](#q-3d53415bc6bd566f8725c41bc1d254ec163cdc021ce675592e7cd60ed395ac56)

FACT: 2022 A pitching, cutoff 2022-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "7dee20ee8c9f3e521c913aac4cef72ef1c74666411f46e05762ea40fc43b0ac8", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e9b5c019c9da](#q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a), [Q-7dee20ee8c9f](#q-7dee20ee8c9f3e521c913aac4cef72ef1c74666411f46e05762ea40fc43b0ac8)

FACT: 2022 A pitching, cutoff 2022-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2022-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "f0eaaded9d2a65612dfab5ed5c0654a82580436e1585a95c0cebe87fd8439392", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-e9b5c019c9da](#q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a), [Q-f0eaaded9d2a](#q-f0eaaded9d2a65612dfab5ed5c0654a82580436e1585a95c0cebe87fd8439392)

FACT: 2023 AAA hitting, person 669899: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-c632a9d99840](#q-c632a9d998407711ae2bfc9740d7253089ec5350d93ae72dfbaedb3923293a8f), [Q-e10fd3f756d5](#q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a)

FACT: 2023 AAA hitting log/season additive agreement: `{}`. [Q-e10fd3f756d5](#q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a), [Q-ef06abb34fb0](#q-ef06abb34fb0b94162764a26b47dfbdaf9e86fda0a19c4b7c7f6c1f6dadb41d4)

FACT: 2023 AAA hitting, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "8fd8dd4d2ca06ebd882a8d591376d6239e6537edbb565eac43fa531f3a0b77d6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-e10fd3f756d5](#q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a), [Q-8fd8dd4d2ca0](#q-8fd8dd4d2ca06ebd882a8d591376d6239e6537edbb565eac43fa531f3a0b77d6)

FACT: 2023 AAA hitting, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "e150918998897eb66a0d2210f82fe8e5604c127a5946d96c32f86d43eda09283", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-e10fd3f756d5](#q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a), [Q-e15091899889](#q-e150918998897eb66a0d2210f82fe8e5604c127a5946d96c32f86d43eda09283)

FACT: 2023 AAA hitting, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "4ff7a4accb5f7f868fee9a9b084e565c4c3f782d5ff960df6a2dd1c1f922e792", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-e10fd3f756d5](#q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a), [Q-4ff7a4accb5f](#q-4ff7a4accb5f7f868fee9a9b084e565c4c3f782d5ff960df6a2dd1c1f922e792)

FACT: 2023 AAA pitching, person 685126: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-9c37e9cb090b](#q-9c37e9cb090b7c33a13c244681de374f6f48538c118c2556c64cc84257208292), [Q-9c024bff08a4](#q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c)

FACT: 2023 AAA pitching log/season additive agreement: `{}`. [Q-9c024bff08a4](#q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c), [Q-bb33d73d5dcc](#q-bb33d73d5dcc8025e602c35605ca73e33b80dbaad83aa50cd1856a99edeb3269)

FACT: 2023 AAA pitching, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "d93287baab285934e0567a99e7b0ec724608ca3255b09edd73e260c58761180c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-9c024bff08a4](#q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c), [Q-d93287baab28](#q-d93287baab285934e0567a99e7b0ec724608ca3255b09edd73e260c58761180c)

FACT: 2023 AAA pitching, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "7c9bb497c73c7efefbb22e8f5ab5001c9cb08a07c85a317190a53b03fe7fad2c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-9c024bff08a4](#q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c), [Q-7c9bb497c73c](#q-7c9bb497c73c7efefbb22e8f5ab5001c9cb08a07c85a317190a53b03fe7fad2c)

FACT: 2023 AAA pitching, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "053df08f6d6f41c0a34b2b366926f5407b8e644a7e3812926532481f10cb0bfc", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-9c024bff08a4](#q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c), [Q-053df08f6d6f](#q-053df08f6d6f41c0a34b2b366926f5407b8e644a7e3812926532481f10cb0bfc)

FACT: 2023 AA hitting, person 695462: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-2a5edc5754cc](#q-2a5edc5754cc69b843abf09fd23bee6f740b405c2d9b9b34fba7a3de891510bc), [Q-f614797863ae](#q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd)

FACT: 2023 AA hitting log/season additive agreement: `{}`. [Q-f614797863ae](#q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd), [Q-a72e5bbaf542](#q-a72e5bbaf54220326ec4c28671df425246aa30c6d415da4e732c6e4b6fea39b2)

FACT: 2023 AA hitting, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "38c40f312ec9d31aac482e26c4a4d11e86409c210e16eed6d136c7a0cbd1eaec", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-f614797863ae](#q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd), [Q-38c40f312ec9](#q-38c40f312ec9d31aac482e26c4a4d11e86409c210e16eed6d136c7a0cbd1eaec)

FACT: 2023 AA hitting, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "5d25cd7a4483fafa0f5b4e29c9c3b718b309aa5be8a33d2541b53e1af715261e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-f614797863ae](#q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd), [Q-5d25cd7a4483](#q-5d25cd7a4483fafa0f5b4e29c9c3b718b309aa5be8a33d2541b53e1af715261e)

FACT: 2023 AA hitting, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "8fc2b76d01d7e9049fe5e31b4c25888257e74d4439ba46543a3608ead0ed2f89", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-f614797863ae](#q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd), [Q-8fc2b76d01d7](#q-8fc2b76d01d7e9049fe5e31b4c25888257e74d4439ba46543a3608ead0ed2f89)

FACT: 2023 AA pitching, person 674047: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-4e98d7554ff0](#q-4e98d7554ff0bf406e9621991cf423649225dbb14a4f4aea22cc472d20e1c6c2), [Q-d8d19b146be8](#q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461)

FACT: 2023 AA pitching log/season additive agreement: `{}`. [Q-d8d19b146be8](#q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461), [Q-da6aa628cbaa](#q-da6aa628cbaad9d4e7cdd4700f29059c9a42d5eb918a921676e934a42604583d)

FACT: 2023 AA pitching, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dce9dc3efb4871a1a7609d736002c8d904329c27a94766239569c88e33b82f92", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d8d19b146be8](#q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461), [Q-dce9dc3efb48](#q-dce9dc3efb4871a1a7609d736002c8d904329c27a94766239569c88e33b82f92)

FACT: 2023 AA pitching, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "4c9d35cdc83a5963bf2d21d3889359a174e7868763e79a8f73e6e71eff9bd3ef", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d8d19b146be8](#q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461), [Q-4c9d35cdc83a](#q-4c9d35cdc83a5963bf2d21d3889359a174e7868763e79a8f73e6e71eff9bd3ef)

FACT: 2023 AA pitching, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "b0384acb558007bdbe1640120727cb915461f61a26a2b17af0185a26b5313bf3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-d8d19b146be8](#q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461), [Q-b0384acb5580](#q-b0384acb558007bdbe1640120727cb915461f61a26a2b17af0185a26b5313bf3)

FACT: 2023 A+ hitting, person 687529: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-f1f97bfe5707](#q-f1f97bfe5707d90baaccb777624b18399efbfc8e66176d7d28fdf66559a85210), [Q-d1c462101f53](#q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944)

FACT: 2023 A+ hitting log/season additive agreement: `{}`. [Q-d1c462101f53](#q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944), [Q-2f7fc438780a](#q-2f7fc438780a902490e861550c768e61c8fcadbfaf8ebac8d551a5d06c8cbecc)

FACT: 2023 A+ hitting, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "e09d5ab6bb8e2767cd433e00ac8a5631e2eaf370e1f39bd3afede0ece6fd4c09", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d1c462101f53](#q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944), [Q-e09d5ab6bb8e](#q-e09d5ab6bb8e2767cd433e00ac8a5631e2eaf370e1f39bd3afede0ece6fd4c09)

FACT: 2023 A+ hitting, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f7e6f0d85bd65471ac3aa650e94f0029d88b30c9a8afd110d69f91955068b8ee", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d1c462101f53](#q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944), [Q-f7e6f0d85bd6](#q-f7e6f0d85bd65471ac3aa650e94f0029d88b30c9a8afd110d69f91955068b8ee)

FACT: 2023 A+ hitting, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "315abcf53b2250051da51e06f208d43a78b47a18b80c05850082ca393252e092", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-d1c462101f53](#q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944), [Q-315abcf53b22](#q-315abcf53b2250051da51e06f208d43a78b47a18b80c05850082ca393252e092)

FACT: 2023 A+ pitching, person 683409: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-7e916100cb5b](#q-7e916100cb5bf3eb62952b67f421adeac3485d91a73fe92fedf42eddcfc61e37), [Q-689121cd19ff](#q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31)

FACT: 2023 A+ pitching log/season additive agreement: `{}`. [Q-689121cd19ff](#q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31), [Q-6b9f765157c0](#q-6b9f765157c058c19decf2298cb5f23210220ccb6ea0acf521d49c110bf8ceb4)

FACT: 2023 A+ pitching, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dd598791636e0a3b7ff4126680e7852a4dd0b6a10d7f1f46a7bf7d7bce0fedf1", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-689121cd19ff](#q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31), [Q-dd598791636e](#q-dd598791636e0a3b7ff4126680e7852a4dd0b6a10d7f1f46a7bf7d7bce0fedf1)

FACT: 2023 A+ pitching, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "5682065ec207ef39b139bf47ea943966a42c01c08d75aa4dfad80842bb68c624", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-689121cd19ff](#q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31), [Q-5682065ec207](#q-5682065ec207ef39b139bf47ea943966a42c01c08d75aa4dfad80842bb68c624)

FACT: 2023 A+ pitching, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "7efb5d29ef8446c316b7dfe697e0406549b1812ff7339cebf5ccb8f34c24b983", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-689121cd19ff](#q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31), [Q-7efb5d29ef84](#q-7efb5d29ef8446c316b7dfe697e0406549b1812ff7339cebf5ccb8f34c24b983)

FACT: 2023 A hitting, person 689531: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-b7ed33f3430f](#q-b7ed33f3430f6181921b0b6c02eceeffe2df7ad995ac0bf766ea7737e6f4ec32), [Q-c799f9cc7a1b](#q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464)

FACT: 2023 A hitting log/season additive agreement: `{}`. [Q-c799f9cc7a1b](#q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464), [Q-5efab228cb8c](#q-5efab228cb8cba072b539707500f5c8eab36f6b55212c840972b0051879ec48a)

FACT: 2023 A hitting, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "e66123827da88e0e5b1e83161604c5cf2ec098054fb2f0864325b905f250c028", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-c799f9cc7a1b](#q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464), [Q-e66123827da8](#q-e66123827da88e0e5b1e83161604c5cf2ec098054fb2f0864325b905f250c028)

FACT: 2023 A hitting, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "4345112fe3d5e19e3122bbe481073cd597eff165c7d98ae7cea3367f4eac8960", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-c799f9cc7a1b](#q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464), [Q-4345112fe3d5](#q-4345112fe3d5e19e3122bbe481073cd597eff165c7d98ae7cea3367f4eac8960)

FACT: 2023 A hitting, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "6768de1e1ee12da2ab649b5c58bf58f950aedad9e504959effc85233df68d344", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-c799f9cc7a1b](#q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464), [Q-6768de1e1ee1](#q-6768de1e1ee12da2ab649b5c58bf58f950aedad9e504959effc85233df68d344)

FACT: 2023 A pitching, person 695250: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-0d3ceee6dfe6](#q-0d3ceee6dfe69e293091c0145d7815ef980e6c911f280121776e503d52e12a35), [Q-dafaa6625ff3](#q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc)

FACT: 2023 A pitching log/season additive agreement: `{}`. [Q-dafaa6625ff3](#q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc), [Q-df63d9488d33](#q-df63d9488d33d77a4561cdc9f0619dc234c3c9e4bd00189c31290e12d28c4d7c)

FACT: 2023 A pitching, cutoff 2023-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "27a9dde0a377d4cf0552c8350d5d5f5da52058ad40783b29b3497f9a39b5b29b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-dafaa6625ff3](#q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc), [Q-27a9dde0a377](#q-27a9dde0a377d4cf0552c8350d5d5f5da52058ad40783b29b3497f9a39b5b29b)

FACT: 2023 A pitching, cutoff 2023-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "943dca9adaee4c3a890afec48b23148809404fef248d4b3654eba9b2cd8a0017", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-dafaa6625ff3](#q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc), [Q-943dca9adaee](#q-943dca9adaee4c3a890afec48b23148809404fef248d4b3654eba9b2cd8a0017)

FACT: 2023 A pitching, cutoff 2023-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2023-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "25af71cd6fffc0163f7b7ed34a1d2652fc48c33d5d2e585723436c60f2e2c5d2", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-dafaa6625ff3](#q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc), [Q-25af71cd6fff](#q-25af71cd6fffc0163f7b7ed34a1d2652fc48c33d5d2e585723436c60f2e2c5d2)

FACT: 2024 AAA hitting, person 682877: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-0be3b9bfced5](#q-0be3b9bfced55d7fea1fe05011de6be56cd5d93fa515d0af2fdbe6bfa16da8e2), [Q-774a1837da10](#q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6)

FACT: 2024 AAA hitting log/season additive agreement: `{}`. [Q-774a1837da10](#q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6), [Q-1477828214dc](#q-1477828214dc47eb235374c9480c7f1d882cb74a4b8dbefec0b783e137d7f7d2)

FACT: 2024 AAA hitting, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "0d19a54c518df917235e7ee898d8bae09a8ddc7080fe6a989379adebef2bb5a5", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-774a1837da10](#q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6), [Q-0d19a54c518d](#q-0d19a54c518df917235e7ee898d8bae09a8ddc7080fe6a989379adebef2bb5a5)

FACT: 2024 AAA hitting, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "cbb07acf50ee67bef36434ffb411660a217b010ae15fa7735140c7f32342035f", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-774a1837da10](#q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6), [Q-cbb07acf50ee](#q-cbb07acf50ee67bef36434ffb411660a217b010ae15fa7735140c7f32342035f)

FACT: 2024 AAA hitting, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "e3457da070e4e4b32b1c48a7c9605c6c29649a31f867a8fd16ab25ac654fd1a6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-774a1837da10](#q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6), [Q-e3457da070e4](#q-e3457da070e4e4b32b1c48a7c9605c6c29649a31f867a8fd16ab25ac654fd1a6)

UNKNOWN: 2024 AAA pitching: incomplete. [Q-3bfe13436817](#q-3bfe13436817d5295d57348650d13b537718eebe778070484ea92d51a54102ae)

FACT: 2024 AA hitting, person 691442: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-1509354dc23e](#q-1509354dc23ec61e338d9a3ddffa560f8087b3b2ecb4011c9f1dafc1315eff35), [Q-cc29b39f070f](#q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba)

FACT: 2024 AA hitting log/season additive agreement: `{}`. [Q-cc29b39f070f](#q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba), [Q-c391660b5b51](#q-c391660b5b51c6376a92b132f7a66b21465a2c453c32d57cb0bdfef8eb832167)

FACT: 2024 AA hitting, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "67f87ffbeb949e5da966bf9a1d056a8f0a9fe10e7d040aafebbd3d358b4108d0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-cc29b39f070f](#q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba), [Q-67f87ffbeb94](#q-67f87ffbeb949e5da966bf9a1d056a8f0a9fe10e7d040aafebbd3d358b4108d0)

FACT: 2024 AA hitting, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "c4798ad79c5519bde13a754da8273a285cd0c677912e0fa97a6e0d0397b4f0bd", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-cc29b39f070f](#q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba), [Q-c4798ad79c55](#q-c4798ad79c5519bde13a754da8273a285cd0c677912e0fa97a6e0d0397b4f0bd)

FACT: 2024 AA hitting, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "9c7368c811d9b0719469870b80d8de68b6dd9d0f7508db3da2819d067e240d9a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-cc29b39f070f](#q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba), [Q-9c7368c811d9](#q-9c7368c811d9b0719469870b80d8de68b6dd9d0f7508db3da2819d067e240d9a)

FACT: 2024 AA pitching, person 694335: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-b27982099b8b](#q-b27982099b8b3b969be7c08a9748c13c4cb22a8b34765f03541d15e9c50a3f3c), [Q-35db42c0941d](#q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f)

FACT: 2024 AA pitching log/season additive agreement: `{}`. [Q-35db42c0941d](#q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f), [Q-ed4416c37c30](#q-ed4416c37c30f4461779cab1d3953fef4047a8b2c6e680b78b18faa0e7fd1689)

FACT: 2024 AA pitching, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "3cab11cc507793f65d4f321fc3a9996e43969171750e8a1ee200dff482f4252d", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-35db42c0941d](#q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f), [Q-3cab11cc5077](#q-3cab11cc507793f65d4f321fc3a9996e43969171750e8a1ee200dff482f4252d)

FACT: 2024 AA pitching, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "13f3f469fa8cdd01855034ccd79ad3c3e041426eb8afd8d01312c01fa4601932", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-35db42c0941d](#q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f), [Q-13f3f469fa8c](#q-13f3f469fa8cdd01855034ccd79ad3c3e041426eb8afd8d01312c01fa4601932)

FACT: 2024 AA pitching, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dbfdea5b61a7b94b057649b15ab44af83030648c47219aa3138f7e80d09c9b95", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-35db42c0941d](#q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f), [Q-dbfdea5b61a7](#q-dbfdea5b61a7b94b057649b15ab44af83030648c47219aa3138f7e80d09c9b95)

FACT: 2024 A+ hitting, person 695521: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-792bd089985d](#q-792bd089985d7f77cd075bdddfa939d5e4c87e3c18234b124c78448c16175151), [Q-eca4a6ee9f5a](#q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502)

FACT: 2024 A+ hitting log/season additive agreement: `{}`. [Q-eca4a6ee9f5a](#q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502), [Q-db697d69c707](#q-db697d69c7072ba17cdce3d3de49dcef1c32985e435f80a41621f4941fe38f2c)

FACT: 2024 A+ hitting, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "a85cdfd6a265d8e93e8454e0bed51f5c32f461693cbb4801c3194673fd6f2bfb", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-eca4a6ee9f5a](#q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502), [Q-a85cdfd6a265](#q-a85cdfd6a265d8e93e8454e0bed51f5c32f461693cbb4801c3194673fd6f2bfb)

FACT: 2024 A+ hitting, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "c3b5c7d608c793f52660ff0001d54bef0531ae9d8250b9aed9712dd86f6e348b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-eca4a6ee9f5a](#q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502), [Q-c3b5c7d608c7](#q-c3b5c7d608c793f52660ff0001d54bef0531ae9d8250b9aed9712dd86f6e348b)

FACT: 2024 A+ hitting, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "a650ecaaf35af484cbd32440b46d0b17d45399e3ff015c7d299758d891d98e9f", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-eca4a6ee9f5a](#q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502), [Q-a650ecaaf35a](#q-a650ecaaf35af484cbd32440b46d0b17d45399e3ff015c7d299758d891d98e9f)

FACT: 2024 A+ pitching, person 806362: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-11cd6b541afc](#q-11cd6b541afc062f842bd14ff543bc08e98f1c784fddb8d88323a1f8be63341a), [Q-ddb875a08dd6](#q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798)

FACT: 2024 A+ pitching log/season additive agreement: `{}`. [Q-ddb875a08dd6](#q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798), [Q-1d92915686a1](#q-1d92915686a1dc9de6b60fff25819c475678a160ab3f8b4e741d4bfd454a615c)

FACT: 2024 A+ pitching, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "c202bc5df9140d5d3561d21b16b16bcd7e7881b17eb691c404e2f7f5025e9160", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ddb875a08dd6](#q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798), [Q-c202bc5df914](#q-c202bc5df9140d5d3561d21b16b16bcd7e7881b17eb691c404e2f7f5025e9160)

FACT: 2024 A+ pitching, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "b22a61a18c1c0782371ab9dc8830fc6d23c5aa71ecb8f002b62f2e1bec66edbe", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ddb875a08dd6](#q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798), [Q-b22a61a18c1c](#q-b22a61a18c1c0782371ab9dc8830fc6d23c5aa71ecb8f002b62f2e1bec66edbe)

FACT: 2024 A+ pitching, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "ec39f58bde55408975d75d6f506b4dddc69c3e93424830b90008ef5f30e20e28", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ddb875a08dd6](#q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798), [Q-ec39f58bde55](#q-ec39f58bde55408975d75d6f506b4dddc69c3e93424830b90008ef5f30e20e28)

FACT: 2024 A hitting, person 703149: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-c9466fcde417](#q-c9466fcde4175df4878eba8272d607842c90606d16b39dd56ce4669fc46bbce1), [Q-47fcf69fecc4](#q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982)

FACT: 2024 A hitting log/season additive agreement: `{}`. [Q-47fcf69fecc4](#q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982), [Q-7fb8eecdc471](#q-7fb8eecdc4716eb386a50e1436bce29211d088173e87152e51ba081762c248c8)

FACT: 2024 A hitting, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "ed0560c652874b76a7d0458fab0fae4bdc2a7ecb8843ed6c241d0c031ecb111a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-47fcf69fecc4](#q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982), [Q-ed0560c65287](#q-ed0560c652874b76a7d0458fab0fae4bdc2a7ecb8843ed6c241d0c031ecb111a)

FACT: 2024 A hitting, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "778f9922901bce4a3b62b105aad7eb56f67effa1c8f3bac6aafa5ff58a98f5b9", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-47fcf69fecc4](#q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982), [Q-778f9922901b](#q-778f9922901bce4a3b62b105aad7eb56f67effa1c8f3bac6aafa5ff58a98f5b9)

FACT: 2024 A hitting, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "dccab47b0d6c65e08fff1df6e22355106c2b55f1ab1817220d4ccaf91a4bdb97", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-47fcf69fecc4](#q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982), [Q-dccab47b0d6c](#q-dccab47b0d6c65e08fff1df6e22355106c2b55f1ab1817220d4ccaf91a4bdb97)

FACT: 2024 A pitching, person 692624: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-191ba00c8bf2](#q-191ba00c8bf2cc6603356720aa0636306b622de11b1d265c0f82f01d67fb7e59), [Q-3aac98bc3ad8](#q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956)

FACT: 2024 A pitching log/season additive agreement: `{}`. [Q-3aac98bc3ad8](#q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956), [Q-39d1de4b2038](#q-39d1de4b2038eae42e719a53bece450b8a2cc01b9860b9816eea832352d77917)

FACT: 2024 A pitching, cutoff 2024-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "dfdd7cafc10cb292773dda2df2d5e51af6b0669aa9289e2cc706ecdbc0adb931", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3aac98bc3ad8](#q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956), [Q-dfdd7cafc10c](#q-dfdd7cafc10cb292773dda2df2d5e51af6b0669aa9289e2cc706ecdbc0adb931)

FACT: 2024 A pitching, cutoff 2024-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "e1967b9b6bf84f823be3bc0729d658ee94630d284c6d8211009cd0f22fe2ce92", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3aac98bc3ad8](#q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956), [Q-e1967b9b6bf8](#q-e1967b9b6bf84f823be3bc0729d658ee94630d284c6d8211009cd0f22fe2ce92)

FACT: 2024 A pitching, cutoff 2024-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2024-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "4f13475daaeaa1268c11da751c88896c5587cdd6f880194ee7b75ee8eddf3b12", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3aac98bc3ad8](#q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956), [Q-4f13475daaea](#q-4f13475daaeaa1268c11da751c88896c5587cdd6f880194ee7b75ee8eddf3b12)

FACT: 2025 AAA hitting, person 669899: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-8e956eb23007](#q-8e956eb23007daa3bbf0531dc788127d52495ec4a262b1b0708e8cd31e6269b3), [Q-31e3d6b298b9](#q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456)

FACT: 2025 AAA hitting log/season additive agreement: `{}`. [Q-31e3d6b298b9](#q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456), [Q-108fc6908d14](#q-108fc6908d14529ce4a1da59459154ba8503453f0bd1b3472713ca580f33a300)

FACT: 2025 AAA hitting, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "861a6d82009721f158ca10785f1f514a3afe3f8ca58942f042e7792a53248afa", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-31e3d6b298b9](#q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456), [Q-861a6d820097](#q-861a6d82009721f158ca10785f1f514a3afe3f8ca58942f042e7792a53248afa)

FACT: 2025 AAA hitting, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "544b0dd605da8635778e20d03f19c209818a37d08a79721a757b79eb273c13f6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-31e3d6b298b9](#q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456), [Q-544b0dd605da](#q-544b0dd605da8635778e20d03f19c209818a37d08a79721a757b79eb273c13f6)

FACT: 2025 AAA hitting, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "e03f080abd6ec940784bce55132571440ce0e27f65cde2bc427c64ce2c13d464", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-31e3d6b298b9](#q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456), [Q-e03f080abd6e](#q-e03f080abd6ec940784bce55132571440ce0e27f65cde2bc427c64ce2c13d464)

FACT: 2025 AAA pitching, person 665996: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-351e85ce3426](#q-351e85ce3426b3ca936091f49a447bd6413cc850e941116b5dac256c42cc4fbd), [Q-3e7236e6253b](#q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b)

FACT: 2025 AAA pitching log/season additive agreement: `{}`. [Q-3e7236e6253b](#q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b), [Q-e7461681446f](#q-e7461681446f21d2dc51b217d6a2b9d5492baf6f9c6fd67bfddb588c426f9b8e)

FACT: 2025 AAA pitching, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "6ee154426edd4384b72c3026c308390edbd4a8e70bb0278bb11fdf9b54996de6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3e7236e6253b](#q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b), [Q-6ee154426edd](#q-6ee154426edd4384b72c3026c308390edbd4a8e70bb0278bb11fdf9b54996de6)

FACT: 2025 AAA pitching, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "7acd8a03b6030e73c43fe355c526f595656f422926c7c7b8cd8bffde2b55ad22", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3e7236e6253b](#q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b), [Q-7acd8a03b603](#q-7acd8a03b6030e73c43fe355c526f595656f422926c7c7b8cd8bffde2b55ad22)

FACT: 2025 AAA pitching, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "437aa523ef7348fa3c068cc71a3c0489a767c808a43dceff7671a28daa05070a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-3e7236e6253b](#q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b), [Q-437aa523ef73](#q-437aa523ef7348fa3c068cc71a3c0489a767c808a43dceff7671a28daa05070a)

FACT: 2025 AA hitting, person 800325: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-9aecef586a68](#q-9aecef586a685812354a731bacd817e16d85a755b83de0c6b92918d1dd6ef37e), [Q-5e58cdf8040d](#q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6)

FACT: 2025 AA hitting log/season additive agreement: `{}`. [Q-5e58cdf8040d](#q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6), [Q-2833e489161d](#q-2833e489161d2e59024bcd33d0cce56cb0fd05266849eb3cced26995b81563ca)

FACT: 2025 AA hitting, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "c370e0b57b3e885e065ea7a5e8552a6af27490d2ad885c2bc13876437189f836", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5e58cdf8040d](#q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6), [Q-c370e0b57b3e](#q-c370e0b57b3e885e065ea7a5e8552a6af27490d2ad885c2bc13876437189f836)

FACT: 2025 AA hitting, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "8d7a464ece7c0f7f894437d8af4b1c8e78ba7c9d51118a62e6ab9ea786e800e0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5e58cdf8040d](#q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6), [Q-8d7a464ece7c](#q-8d7a464ece7c0f7f894437d8af4b1c8e78ba7c9d51118a62e6ab9ea786e800e0)

FACT: 2025 AA hitting, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "6f36fe335696007de991c0a712a3a381f2a3cbe8bfa8fd780f0ea2261e02c26c", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-5e58cdf8040d](#q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6), [Q-6f36fe335696](#q-6f36fe335696007de991c0a712a3a381f2a3cbe8bfa8fd780f0ea2261e02c26c)

FACT: 2025 AA pitching, person 681744: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-27ab2d8b4c42](#q-27ab2d8b4c421ada4df4cdc01f00a4250f43d210bd9ca3bb9c7dc1fa5f008834), [Q-ac2d070af302](#q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5)

FACT: 2025 AA pitching log/season additive agreement: `{}`. [Q-ac2d070af302](#q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5), [Q-603ed2b79ba8](#q-603ed2b79ba8fa883375ee775564fff56295019fde19f03df4ace921d6395b9e)

FACT: 2025 AA pitching, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "0492691e573da6a4178e085bd8715285541ad4a2ce5065a0fbb0ea862de4560f", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ac2d070af302](#q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5), [Q-0492691e573d](#q-0492691e573da6a4178e085bd8715285541ad4a2ce5065a0fbb0ea862de4560f)

FACT: 2025 AA pitching, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "89c46961b3ff6e2f5734f40a8a0fa9ba9b17499dd6a98962e19e82bb7e02903e", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ac2d070af302](#q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5), [Q-89c46961b3ff](#q-89c46961b3ff6e2f5734f40a8a0fa9ba9b17499dd6a98962e19e82bb7e02903e)

FACT: 2025 AA pitching, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "ca881ab45163475157d599ffe1cda07b3e80551dd93ca0adc6a87b73b9c0a7ea", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-ac2d070af302](#q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5), [Q-ca881ab45163](#q-ca881ab45163475157d599ffe1cda07b3e80551dd93ca0adc6a87b73b9c0a7ea)

FACT: 2025 A+ hitting, person 813841: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-d19ea932a5e8](#q-d19ea932a5e849689f18ce7c14ff03da8946be4bd8c1fce7ffc1b755b50df0ea), [Q-aaaac569e68a](#q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236)

FACT: 2025 A+ hitting log/season additive agreement: `{}`. [Q-aaaac569e68a](#q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236), [Q-06994f76c3d9](#q-06994f76c3d9a8acaa930f1bdc4dd63000805fa7abd77158aea82b9ec44a2f7a)

FACT: 2025 A+ hitting, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f55dece0bde881ef2d2c4c2621aefd2ff38801bc79fe522976c9a58111d99eb6", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-aaaac569e68a](#q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236), [Q-f55dece0bde8](#q-f55dece0bde881ef2d2c4c2621aefd2ff38801bc79fe522976c9a58111d99eb6)

FACT: 2025 A+ hitting, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "cbbfba42feab6dc155e659257777828783dcc59ec21bd87a25322407bff6040b", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-aaaac569e68a](#q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236), [Q-cbbfba42feab](#q-cbbfba42feab6dc155e659257777828783dcc59ec21bd87a25322407bff6040b)

FACT: 2025 A+ hitting, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "cb3762f69b59af74fa08eb0e32b2be0a1a89a7fa4e6d2a59c9cb4cdb3b34a3c1", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-aaaac569e68a](#q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236), [Q-cb3762f69b59](#q-cb3762f69b59af74fa08eb0e32b2be0a1a89a7fa4e6d2a59c9cb4cdb3b34a3c1)

FACT: 2025 A+ pitching, person 681060: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-99dd5fa81ab2](#q-99dd5fa81ab203fa5d5b35daf5a03743534d2bf8b507b8bcbdc69e0ab7f6c44f), [Q-cfa54bef48c6](#q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc)

FACT: 2025 A+ pitching log/season additive agreement: `{}`. [Q-cfa54bef48c6](#q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc), [Q-5825a4a809f2](#q-5825a4a809f239e719363f8bc0f629b05588bc6881750daf88cdbd6d4688aac3)

FACT: 2025 A+ pitching, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "1ebcee9b128bdae645669f279caac0b281415e4d0f8073d3fbde4ba893400b51", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cfa54bef48c6](#q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc), [Q-1ebcee9b128b](#q-1ebcee9b128bdae645669f279caac0b281415e4d0f8073d3fbde4ba893400b51)

FACT: 2025 A+ pitching, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "b9fbd9f2b04d32ab47b0668696423f0a2586743c41839c8ba5da157dc9b629b0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cfa54bef48c6](#q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc), [Q-b9fbd9f2b04d](#q-b9fbd9f2b04d32ab47b0668696423f0a2586743c41839c8ba5da157dc9b629b0)

FACT: 2025 A+ pitching, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "df151c6aab5be1241bdfd49e3719d9c7745ac8c85b1ed270d9bf4dc3bd62f5c0", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-cfa54bef48c6](#q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc), [Q-df151c6aab5b](#q-df151c6aab5be1241bdfd49e3719d9c7745ac8c85b1ed270d9bf4dc3bd62f5c0)

FACT: 2025 A hitting, person 804558: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}`. [Q-1f10fbfb1d28](#q-1f10fbfb1d281563010ff4174c8e7c684565c6c03f938bd7fe043b63b6effc1e), [Q-8aaea0b14b9e](#q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62)

FACT: 2025 A hitting log/season additive agreement: `{}`. [Q-8aaea0b14b9e](#q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62), [Q-3d2b0daa4330](#q-3d2b0daa433051cc3b1b30ce99d333b14b7f6e56c7037b1b43fef4ebcb97ee61)

FACT: 2025 A hitting, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "75f9564e2a041699c9d390b845fbf22a30819444d8311165ad66115f98d58ebb", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-8aaea0b14b9e](#q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62), [Q-75f9564e2a04](#q-75f9564e2a041699c9d390b845fbf22a30819444d8311165ad66115f98d58ebb)

FACT: 2025 A hitting, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "6f985be453fbd299a636079fceedeb5f00d44bdf4fd3851fdcf9ba0307446e3d", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-8aaea0b14b9e](#q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62), [Q-6f985be453fb](#q-6f985be453fbd299a636079fceedeb5f00d44bdf4fd3851fdcf9ba0307446e3d)

FACT: 2025 A hitting, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}, "query_id": "f95b274d6b6bddff94664b2f54d36231082b0a075230c623495adc5463cf4fd3", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"atBats": null, "baseOnBalls": null, "computedAVG": null, "gamesPlayed": null, "hitByPitch": null, "hits": null, "plateAppearances": null, "sacFlies": null, "totalBases": null}}, "same_records": true}`. [Q-8aaea0b14b9e](#q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62), [Q-f95b274d6b6b](#q-f95b274d6b6bddff94664b2f54d36231082b0a075230c623495adc5463cf4fd3)

FACT: 2025 A pitching, person 813707: `{"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}`. [Q-4d8cacbea51a](#q-4d8cacbea51afee8bd7a1282074c110a9e7623a6f38d3ab1a79e4f0dae578900), [Q-0037c4d05cad](#q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24)

FACT: 2025 A pitching log/season additive agreement: `{}`. [Q-0037c4d05cad](#q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24), [Q-4655f36bffa8](#q-4655f36bffa8017ab154fbfd02cca63105e66b39af7fb322ad0723902eb4d5f5)

FACT: 2025 A pitching, cutoff 2025-05-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-05-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "4beb815804b6dc083de25306f2f4340d487916cc30ec2ae7558293b4f3b2934a", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0037c4d05cad](#q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24), [Q-4beb815804b6](#q-4beb815804b6dc083de25306f2f4340d487916cc30ec2ae7558293b4f3b2934a)

FACT: 2025 A pitching, cutoff 2025-07-15: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-07-15", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "393f39d43381e27debf1a293f5e1e8d54bbaf34387ac7c2c756f9d5ad8c8e119", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0037c4d05cad](#q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24), [Q-393f39d43381](#q-393f39d43381e27debf1a293f5e1e8d54bbaf34387ac7c2c756f9d5ad8c8e119)

FACT: 2025 A pitching, cutoff 2025-08-31: `{"api_rows": 0, "api_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "date": "2025-08-31", "local_rows": 0, "local_totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}, "query_id": "1e205506291e05d38feb6f717aa012651adcf7ebc1e2b85f130da9f6b432c9b7", "remote_missingness": {"completion_date_rows": 0, "conflicting_game_keys": 0, "earliest_date": null, "exact_duplicates": 0, "latest_date": null, "missing_dates": 0, "missing_game_ids": 0, "rows": 0, "sport_ids": [], "sport_unverified_rows": 0, "team_ids": [], "totals": {"baseOnBalls": null, "earnedRuns": null, "gamesPlayed": null, "hits": null, "outs": null, "strikeOuts": null}}, "same_records": true}`. [Q-0037c4d05cad](#q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24), [Q-1e205506291e](#q-1e205506291e05d38feb6f717aa012651adcf7ebc1e2b85f130da9f6b432c9b7)

UNKNOWN: A game date is not proof that its final statistics were available on that date. Suspended games, later completions, scoring corrections, and publication delays require additional evidence. The probe counts explicit completionDate fields but does not query game feeds to resolve them; strict decision-time availability remains unknown even when date filters agree.

INFERENCE: Cross-level joins should use person ID, season, sport ID, group, and gamePk/date. Team changes within one level are not necessarily level changes. Sport IDs are discovered from current names; historical taxonomy and reorganizations require validation. Missing sport tags leave level filter semantics unverified.

## Joins, aggregation, and remaining limits

INFERENCE: Join `transactions.person.id` to `people.id` and `people/{id}/stats`; sample splits use `player.id`. Club keys are `fromTeam.id`, `toTeam.id`, and stats `team.id`. Names are display data, never identity keys. Person lookups must return the requested ID.

FACT: The parser rejects mismatched seasons/sport tags and malformed counts. It removes exact duplicate splits, prefers a single aggregate over team splits, and withholds totals for conflicting multiple aggregates/team rows. Missing fields remain null. IP source strings are retained and converted to outs, never decimal innings. Batting average is recomputed from hits and at-bats when present; other rate statistics are not combined.

UNKNOWN: Aggregate-row conventions, events absent from the API, minor sports not exposed by current discovery, phase ambiguities, and historical corrections prevent a full feasibility claim. No product selection is made.

## Diagnostics

FACT: Failed operation `milbfa 2021 transactions 2021-11-05 through 2021-11-11`: possible truncation: 1000 or more transaction rows

FACT: Failed operation `milbfa 2022 transactions 2022-11-05 through 2022-11-11`: possible truncation: 1000 or more transaction rows

FACT: Failed operation `milbfa 2023 transactions 2023-11-05 through 2023-11-11`: possible truncation: 1000 or more transaction rows

FACT: Failed operation `milbfa 2025 transactions 2025-11-05 through 2025-11-11`: possible truncation: 1000 or more transaction rows

FACT: Failed operation `CALLUP 2019 AAA hitting`: Game-log sport filter ignored

FACT: Failed operation `CALLUP 2022 AAA hitting`: Game-log sport filter ignored

FACT: Failed operation `CALLUP 2024 AAA pitching`: Game-log sport filter ignored

## Executed query ledger

<a id="q-8e7729dddc2eb200ca8a1ea24efbef2e2475afce191b3c6dcfc238d8dac68c10"></a>

FACT: Q-8e7729dddc2e: [GET sports](https://statsapi.mlb.com/api/v1/sports); parameters=`{}`; status=ok; source=network; requested UTC=2026-10-08T19:34:01.646185Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:01.646212Z"}]`. Manifest query ID `8e7729dddc2eb200ca8a1ea24efbef2e2475afce191b3c6dcfc238d8dac68c10`.

<a id="q-aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184"></a>

FACT: Q-aed565e5a4b5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-01&endDate=2018-10-07); parameters=`{"endDate": "2018-10-07", "startDate": "2018-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:01.777837Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:02.146296Z"}]`. Manifest query ID `aed565e5a4b5c7c4f1ed601b1e8bec6958f88a5af0813a9502c28e80ac8da184`.

<a id="q-c03218652ce0480f51bbb4db98f0a96aa196af4c9ad1191bc251d04e5357d0f2"></a>

FACT: Q-c03218652ce0: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-08&endDate=2018-10-14); parameters=`{"endDate": "2018-10-14", "startDate": "2018-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:02.300585Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:02.646379Z"}]`. Manifest query ID `c03218652ce0480f51bbb4db98f0a96aa196af4c9ad1191bc251d04e5357d0f2`.

<a id="q-b7664bf52b22c3b3a7919bb8eed42f4fa2e2c402decfe38ce1d7c9c7e49b6e3f"></a>

FACT: Q-b7664bf52b22: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-15&endDate=2018-10-21); parameters=`{"endDate": "2018-10-21", "startDate": "2018-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:03.113605Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:03.146455Z"}]`. Manifest query ID `b7664bf52b22c3b3a7919bb8eed42f4fa2e2c402decfe38ce1d7c9c7e49b6e3f`.

<a id="q-d90422a81f2fddace1cb164746fb731f9dafb060ef9988aaaa88902172b1193f"></a>

FACT: Q-d90422a81f2f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-22&endDate=2018-10-28); parameters=`{"endDate": "2018-10-28", "startDate": "2018-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:03.338787Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:03.646529Z"}]`. Manifest query ID `d90422a81f2fddace1cb164746fb731f9dafb060ef9988aaaa88902172b1193f`.

<a id="q-279dbb174b5b4d589073f5af74e22dab639dd042b5550d0486985e9aff2f9b93"></a>

FACT: Q-279dbb174b5b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-29&endDate=2018-11-04); parameters=`{"endDate": "2018-11-04", "startDate": "2018-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:03.812640Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:04.146604Z"}]`. Manifest query ID `279dbb174b5b4d589073f5af74e22dab639dd042b5550d0486985e9aff2f9b93`.

<a id="q-a59a50f57cb194ea2b8169178097be01dd309cfc1112097fb80ed1b553b57fc5"></a>

FACT: Q-a59a50f57cb1: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-10-29&endDate=2018-11-01); parameters=`{"endDate": "2018-11-01", "startDate": "2018-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:04.419208Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:04.646682Z"}]`. Manifest query ID `a59a50f57cb194ea2b8169178097be01dd309cfc1112097fb80ed1b553b57fc5`.

<a id="q-39083b93e9bb27ccf9d6691cae21aed2cedfb27808a3880238651c8b3f714513"></a>

FACT: Q-39083b93e9bb: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-11-02&endDate=2018-11-04); parameters=`{"endDate": "2018-11-04", "startDate": "2018-11-02"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:04.747977Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:05.146760Z"}]`. Manifest query ID `39083b93e9bb27ccf9d6691cae21aed2cedfb27808a3880238651c8b3f714513`.

<a id="q-6b3dd0bd91715b09510b8822d51393d22c5abc1f06d55722e061c8c0645b6efd"></a>

FACT: Q-6b3dd0bd9171: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-11-05&endDate=2018-11-11); parameters=`{"endDate": "2018-11-11", "startDate": "2018-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:05.262229Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:05.646833Z"}]`. Manifest query ID `6b3dd0bd91715b09510b8822d51393d22c5abc1f06d55722e061c8c0645b6efd`.

<a id="q-1205574dbaabc11ecc91fc75d82dc7d7874428d14bb8f3ee08fa649918359f2e"></a>

FACT: Q-1205574dbaab: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-11-12&endDate=2018-11-18); parameters=`{"endDate": "2018-11-18", "startDate": "2018-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:05.778690Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:06.146908Z"}]`. Manifest query ID `1205574dbaabc11ecc91fc75d82dc7d7874428d14bb8f3ee08fa649918359f2e`.

<a id="q-062e68ae7ce756fb6081884a3d8fc0e7396d35a4d62d548277ab17a413f4a744"></a>

FACT: Q-062e68ae7ce7: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-11-19&endDate=2018-11-25); parameters=`{"endDate": "2018-11-25", "startDate": "2018-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:06.315455Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:06.646985Z"}]`. Manifest query ID `062e68ae7ce756fb6081884a3d8fc0e7396d35a4d62d548277ab17a413f4a744`.

<a id="q-6cddebe915c5b15472b20c9f6a3825210a9beb976a0e32e30b625ef9ba232e85"></a>

FACT: Q-6cddebe915c5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-11-26&endDate=2018-12-02); parameters=`{"endDate": "2018-12-02", "startDate": "2018-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:06.774773Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:07.147038Z"}]`. Manifest query ID `6cddebe915c5b15472b20c9f6a3825210a9beb976a0e32e30b625ef9ba232e85`.

<a id="q-909b90901260f066053a15b0db50de0ba6b851a8e299a3af421286e1d4b01f32"></a>

FACT: Q-909b90901260: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-03&endDate=2018-12-09); parameters=`{"endDate": "2018-12-09", "startDate": "2018-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:07.262279Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:07.647115Z"}]`. Manifest query ID `909b90901260f066053a15b0db50de0ba6b851a8e299a3af421286e1d4b01f32`.

<a id="q-b37bb46bc24bf7ac4d9743ddf810bd6503d84476ff9a18ce41aedd8d7c7c3627"></a>

FACT: Q-b37bb46bc24b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-10&endDate=2018-12-16); parameters=`{"endDate": "2018-12-16", "startDate": "2018-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:07.737269Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:08.147191Z"}]`. Manifest query ID `b37bb46bc24bf7ac4d9743ddf810bd6503d84476ff9a18ce41aedd8d7c7c3627`.

<a id="q-0422b52dd33ebe5663fbb23768d8dc5d6ca26feed05788f1111e75f449d8df6c"></a>

FACT: Q-0422b52dd33e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-17&endDate=2018-12-23); parameters=`{"endDate": "2018-12-23", "startDate": "2018-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:08.250132Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:08.647266Z"}]`. Manifest query ID `0422b52dd33ebe5663fbb23768d8dc5d6ca26feed05788f1111e75f449d8df6c`.

<a id="q-f3fe4646a49c81eeb83c48660ef09aef9a62c1e8328472e478e356ced99880ef"></a>

FACT: Q-f3fe4646a49c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-24&endDate=2018-12-30); parameters=`{"endDate": "2018-12-30", "startDate": "2018-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:08.751926Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:09.147340Z"}]`. Manifest query ID `f3fe4646a49c81eeb83c48660ef09aef9a62c1e8328472e478e356ced99880ef`.

<a id="q-2bc8615ebd52a9fa45e78d814b146ae00f5b2912e6bcd35a9bf2c953c7deb653"></a>

FACT: Q-2bc8615ebd52: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-31&endDate=2018-12-31); parameters=`{"endDate": "2018-12-31", "startDate": "2018-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:09.245629Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:09.647419Z"}]`. Manifest query ID `2bc8615ebd52a9fa45e78d814b146ae00f5b2912e6bcd35a9bf2c953c7deb653`.

<a id="q-1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b"></a>

FACT: Q-1c091429c788: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-01&endDate=2019-10-07); parameters=`{"endDate": "2019-10-07", "startDate": "2019-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:11.424572Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:11.424601Z"}]`. Manifest query ID `1c091429c7880938dd012aae4c53b84659591a4e27f009c30d6d1e9689327f3b`.

<a id="q-d8691442dae22b595b47ea6237f8109fd165ee1846effe27940acbf74c2bedf1"></a>

FACT: Q-d8691442dae2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-08&endDate=2019-10-14); parameters=`{"endDate": "2019-10-14", "startDate": "2019-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:11.545192Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:11.924686Z"}]`. Manifest query ID `d8691442dae22b595b47ea6237f8109fd165ee1846effe27940acbf74c2bedf1`.

<a id="q-5c26dd158fd8203d5b42c1ab20dcd5f14ff5868622277d4960585ff09b47215d"></a>

FACT: Q-5c26dd158fd8: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-15&endDate=2019-10-21); parameters=`{"endDate": "2019-10-21", "startDate": "2019-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:12.059939Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:12.424760Z"}]`. Manifest query ID `5c26dd158fd8203d5b42c1ab20dcd5f14ff5868622277d4960585ff09b47215d`.

<a id="q-f7927f5b7394c9212970db9bb0503e2f62aaf27aabb7f0c3d1939ae36785f80b"></a>

FACT: Q-f7927f5b7394: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-22&endDate=2019-10-28); parameters=`{"endDate": "2019-10-28", "startDate": "2019-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:12.507634Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:12.924839Z"}]`. Manifest query ID `f7927f5b7394c9212970db9bb0503e2f62aaf27aabb7f0c3d1939ae36785f80b`.

<a id="q-5187527003f9aaa1e8b8c871360bd8bd9275f29a05e1b798933a1b0d71cc62ee"></a>

FACT: Q-5187527003f9: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-29&endDate=2019-11-04); parameters=`{"endDate": "2019-11-04", "startDate": "2019-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:13.019350Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:13.424911Z"}]`. Manifest query ID `5187527003f9aaa1e8b8c871360bd8bd9275f29a05e1b798933a1b0d71cc62ee`.

<a id="q-3fe1e3efe926b8b0d4a91180a3664eabcf9fac5a837b2e9876c5bce65d60e51c"></a>

FACT: Q-3fe1e3efe926: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-10-29&endDate=2019-11-01); parameters=`{"endDate": "2019-11-01", "startDate": "2019-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:13.609587Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:13.924988Z"}]`. Manifest query ID `3fe1e3efe926b8b0d4a91180a3664eabcf9fac5a837b2e9876c5bce65d60e51c`.

<a id="q-a1cfd4e9b00ac62eda09d118666f6ccee80e3fda9824831297d73a8eb18847c3"></a>

FACT: Q-a1cfd4e9b00a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-02&endDate=2019-11-04); parameters=`{"endDate": "2019-11-04", "startDate": "2019-11-02"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:14.036201Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:14.425045Z"}]`. Manifest query ID `a1cfd4e9b00ac62eda09d118666f6ccee80e3fda9824831297d73a8eb18847c3`.

<a id="q-38f6349c5d5a0dcda89a6e3762b14678de11db84904796a4938850f8c969f2d3"></a>

FACT: Q-38f6349c5d5a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-05&endDate=2019-11-11); parameters=`{"endDate": "2019-11-11", "startDate": "2019-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:14.584397Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:14.925123Z"}]`. Manifest query ID `38f6349c5d5a0dcda89a6e3762b14678de11db84904796a4938850f8c969f2d3`.

<a id="q-b4b6b728c28f88058bda44cf83e82e2e0054c03f4ffc2d554044f5eb8ba49748"></a>

FACT: Q-b4b6b728c28f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-12&endDate=2019-11-18); parameters=`{"endDate": "2019-11-18", "startDate": "2019-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:15.047742Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:15.425174Z"}]`. Manifest query ID `b4b6b728c28f88058bda44cf83e82e2e0054c03f4ffc2d554044f5eb8ba49748`.

<a id="q-42e6415f2e058a491f3ba53ef63220c82014ec1cf49a776f89ea1b0dc2d25508"></a>

FACT: Q-42e6415f2e05: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-19&endDate=2019-11-25); parameters=`{"endDate": "2019-11-25", "startDate": "2019-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:15.529312Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:15.925290Z"}]`. Manifest query ID `42e6415f2e058a491f3ba53ef63220c82014ec1cf49a776f89ea1b0dc2d25508`.

<a id="q-919d942aee402fc83806567aa08dcb0ea2e3e560b169c1b500c8c5e0219d7120"></a>

FACT: Q-919d942aee40: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-19&endDate=2019-11-22); parameters=`{"endDate": "2019-11-22", "startDate": "2019-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:16.076511Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:16.425362Z"}]`. Manifest query ID `919d942aee402fc83806567aa08dcb0ea2e3e560b169c1b500c8c5e0219d7120`.

<a id="q-2f56b71f4d14c2f355c5e7435a1e6287abe71d7f403539f36703d9625afe1d5e"></a>

FACT: Q-2f56b71f4d14: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-19&endDate=2019-11-20); parameters=`{"endDate": "2019-11-20", "startDate": "2019-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:16.614061Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:16.925438Z"}]`. Manifest query ID `2f56b71f4d14c2f355c5e7435a1e6287abe71d7f403539f36703d9625afe1d5e`.

<a id="q-11a7b0236a762ab622eebeb2085cc7a6f85f1208166ece85ff44e22125dcfb87"></a>

FACT: Q-11a7b0236a76: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-19&endDate=2019-11-19); parameters=`{"endDate": "2019-11-19", "startDate": "2019-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:17.097204Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:17.425513Z"}]`. Manifest query ID `11a7b0236a762ab622eebeb2085cc7a6f85f1208166ece85ff44e22125dcfb87`.

<a id="q-d8b9ba6fd1c57fe80b08081f2cd0234a4237a74dded7a5dec2bba20bace5d610"></a>

FACT: Q-d8b9ba6fd1c5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-20&endDate=2019-11-20); parameters=`{"endDate": "2019-11-20", "startDate": "2019-11-20"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:17.515528Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:17.925589Z"}]`. Manifest query ID `d8b9ba6fd1c57fe80b08081f2cd0234a4237a74dded7a5dec2bba20bace5d610`.

<a id="q-745cbd00473b51d3ebff0026b7a74d1149334e908d9a22a86f9c54483f86f526"></a>

FACT: Q-745cbd00473b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-21&endDate=2019-11-22); parameters=`{"endDate": "2019-11-22", "startDate": "2019-11-21"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:18.084236Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:18.425661Z"}]`. Manifest query ID `745cbd00473b51d3ebff0026b7a74d1149334e908d9a22a86f9c54483f86f526`.

<a id="q-e89b0347f414211bb242dd47d96a46652d57f53bc3706222f7f4241a5480c31f"></a>

FACT: Q-e89b0347f414: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-23&endDate=2019-11-25); parameters=`{"endDate": "2019-11-25", "startDate": "2019-11-23"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:18.498571Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:18.925732Z"}]`. Manifest query ID `e89b0347f414211bb242dd47d96a46652d57f53bc3706222f7f4241a5480c31f`.

<a id="q-8d0feb18d7da8a95577c828d4f3e765388bee0e8e0e8ebc5ab1f91b1ff6ce4b2"></a>

FACT: Q-8d0feb18d7da: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-11-26&endDate=2019-12-02); parameters=`{"endDate": "2019-12-02", "startDate": "2019-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:19.011864Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:19.425766Z"}]`. Manifest query ID `8d0feb18d7da8a95577c828d4f3e765388bee0e8e0e8ebc5ab1f91b1ff6ce4b2`.

<a id="q-202f55d5d7cc27b623dd4f91160f0880c2e09e9a250fdddcfed8cdcb4761d08b"></a>

FACT: Q-202f55d5d7cc: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-03&endDate=2019-12-09); parameters=`{"endDate": "2019-12-09", "startDate": "2019-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:19.518712Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:19.925847Z"}]`. Manifest query ID `202f55d5d7cc27b623dd4f91160f0880c2e09e9a250fdddcfed8cdcb4761d08b`.

<a id="q-7d06f23eab07cd3c306c1f03c76ce11b61b2a6297ed98e950f16cb92d08248c6"></a>

FACT: Q-7d06f23eab07: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-10&endDate=2019-12-16); parameters=`{"endDate": "2019-12-16", "startDate": "2019-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:20.024131Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:20.425935Z"}]`. Manifest query ID `7d06f23eab07cd3c306c1f03c76ce11b61b2a6297ed98e950f16cb92d08248c6`.

<a id="q-76461ee862d36fbfbe247d2bf135ceb6f947b9d7a197c220187599db35fd42ef"></a>

FACT: Q-76461ee862d3: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-17&endDate=2019-12-23); parameters=`{"endDate": "2019-12-23", "startDate": "2019-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:20.561303Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:20.926019Z"}]`. Manifest query ID `76461ee862d36fbfbe247d2bf135ceb6f947b9d7a197c220187599db35fd42ef`.

<a id="q-526966860412dea6955ee919df31f82723fa020552df65b465a4719bdba80994"></a>

FACT: Q-526966860412: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-24&endDate=2019-12-30); parameters=`{"endDate": "2019-12-30", "startDate": "2019-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:21.038587Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:21.426098Z"}]`. Manifest query ID `526966860412dea6955ee919df31f82723fa020552df65b465a4719bdba80994`.

<a id="q-578085b6d920f7479d07e049b63d93971245674f0313e42da1af4d2e541fdf62"></a>

FACT: Q-578085b6d920: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-31&endDate=2019-12-31); parameters=`{"endDate": "2019-12-31", "startDate": "2019-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:21.502588Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:21.926174Z"}]`. Manifest query ID `578085b6d920f7479d07e049b63d93971245674f0313e42da1af4d2e541fdf62`.

<a id="q-0e8ec98c132962612d06d83a9c1e3d878740c09f85733e8489954a2510535f3d"></a>

FACT: Q-0e8ec98c1329: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-01&endDate=2021-10-07); parameters=`{"endDate": "2021-10-07", "startDate": "2021-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:26.047255Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:26.047290Z"}]`. Manifest query ID `0e8ec98c132962612d06d83a9c1e3d878740c09f85733e8489954a2510535f3d`.

<a id="q-6cbbfd61fda6e7cf1cf5765f6e89cb337a40d897eb95f93605f90848336b1f41"></a>

FACT: Q-6cbbfd61fda6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-01&endDate=2021-10-04); parameters=`{"endDate": "2021-10-04", "startDate": "2021-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:26.331381Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:26.547370Z"}]`. Manifest query ID `6cbbfd61fda6e7cf1cf5765f6e89cb337a40d897eb95f93605f90848336b1f41`.

<a id="q-b2746cf151db91c75b622e8db60f8ec590143d1ffd88f73f1d19f152e4fba4e6"></a>

FACT: Q-b2746cf151db: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-01&endDate=2021-10-02); parameters=`{"endDate": "2021-10-02", "startDate": "2021-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:26.704919Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:27.047452Z"}]`. Manifest query ID `b2746cf151db91c75b622e8db60f8ec590143d1ffd88f73f1d19f152e4fba4e6`.

<a id="q-c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c"></a>

FACT: Q-c6fa2705661a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-03&endDate=2021-10-04); parameters=`{"endDate": "2021-10-04", "startDate": "2021-10-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:27.129000Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:27.547540Z"}]`. Manifest query ID `c6fa2705661a49e6efa9032c3c16c6b2edf240bf2641af4dbc80779e88a8379c`.

<a id="q-5610dd2522bd958d35c96922fc56c5807d9765e024bfb4aee96cb4a389ccc5bd"></a>

FACT: Q-5610dd2522bd: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-05&endDate=2021-10-07); parameters=`{"endDate": "2021-10-07", "startDate": "2021-10-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:27.673381Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:28.047615Z"}]`. Manifest query ID `5610dd2522bd958d35c96922fc56c5807d9765e024bfb4aee96cb4a389ccc5bd`.

<a id="q-33f0b93d09b321b8335ee9de9bf88697915b670e23ba9111a6ae8c69e73d756b"></a>

FACT: Q-33f0b93d09b3: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-08&endDate=2021-10-14); parameters=`{"endDate": "2021-10-14", "startDate": "2021-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:28.181544Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:28.547692Z"}]`. Manifest query ID `33f0b93d09b321b8335ee9de9bf88697915b670e23ba9111a6ae8c69e73d756b`.

<a id="q-78719e29669fb69ddcc0056b0029f81767188c7e6ddd71f911c93d4f7f5235a4"></a>

FACT: Q-78719e29669f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-15&endDate=2021-10-21); parameters=`{"endDate": "2021-10-21", "startDate": "2021-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:28.677042Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:29.047741Z"}]`. Manifest query ID `78719e29669fb69ddcc0056b0029f81767188c7e6ddd71f911c93d4f7f5235a4`.

<a id="q-99bf09b219e2bdc8b9754191a410b02517364ffb5205f422dcf9912f1de16f04"></a>

FACT: Q-99bf09b219e2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-22&endDate=2021-10-28); parameters=`{"endDate": "2021-10-28", "startDate": "2021-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:29.150041Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:29.547819Z"}]`. Manifest query ID `99bf09b219e2bdc8b9754191a410b02517364ffb5205f422dcf9912f1de16f04`.

<a id="q-54e63e6ceb3f560a225dd1e0caac286181a6c6f4f83f7e81cbaa9a3065a5cbac"></a>

FACT: Q-54e63e6ceb3f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-22&endDate=2021-10-25); parameters=`{"endDate": "2021-10-25", "startDate": "2021-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:29.696949Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:30.047907Z"}]`. Manifest query ID `54e63e6ceb3f560a225dd1e0caac286181a6c6f4f83f7e81cbaa9a3065a5cbac`.

<a id="q-4af43c0804baaffd791630a852bd1c94a319c3ce7b9366690cacf5a7ca28a026"></a>

FACT: Q-4af43c0804ba: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-26&endDate=2021-10-28); parameters=`{"endDate": "2021-10-28", "startDate": "2021-10-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:30.149931Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:30.547983Z"}]`. Manifest query ID `4af43c0804baaffd791630a852bd1c94a319c3ce7b9366690cacf5a7ca28a026`.

<a id="q-19f18aef3082e9082e34016af0a27e666220e1fc39427b5501f1793b918eba32"></a>

FACT: Q-19f18aef3082: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-10-29&endDate=2021-11-04); parameters=`{"endDate": "2021-11-04", "startDate": "2021-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:30.649274Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:31.048042Z"}]`. Manifest query ID `19f18aef3082e9082e34016af0a27e666220e1fc39427b5501f1793b918eba32`.

<a id="q-b8d13536cdc00c494d13d726b0c1cb8613a9c8d4e50818e2d091472e6e69a15f"></a>

FACT: Q-b8d13536cdc0: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-05&endDate=2021-11-11); parameters=`{"endDate": "2021-11-11", "startDate": "2021-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:31.163891Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:31.548114Z"}]`. Manifest query ID `b8d13536cdc00c494d13d726b0c1cb8613a9c8d4e50818e2d091472e6e69a15f`.

<a id="q-fb8b3a48c85916d2c48d0dcb741aba6dd232f654c88744b75723741233de6aef"></a>

FACT: Q-fb8b3a48c859: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-05&endDate=2021-11-08); parameters=`{"endDate": "2021-11-08", "startDate": "2021-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:31.771861Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:32.048197Z"}]`. Manifest query ID `fb8b3a48c85916d2c48d0dcb741aba6dd232f654c88744b75723741233de6aef`.

<a id="q-2f82e5d4ff8e057bb2d156b190f84ba06a385e01d4b464ea40aa35ba671dad07"></a>

FACT: Q-2f82e5d4ff8e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-05&endDate=2021-11-06); parameters=`{"endDate": "2021-11-06", "startDate": "2021-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:32.283309Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:32.548271Z"}]`. Manifest query ID `2f82e5d4ff8e057bb2d156b190f84ba06a385e01d4b464ea40aa35ba671dad07`.

<a id="q-366b4357784c3b83bc397aee5ad4353f7a65bf5e91282d8be53e6564dd0bc029"></a>

FACT: Q-366b4357784c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-07&endDate=2021-11-08); parameters=`{"endDate": "2021-11-08", "startDate": "2021-11-07"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:32.681381Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:33.048344Z"}]`. Manifest query ID `366b4357784c3b83bc397aee5ad4353f7a65bf5e91282d8be53e6564dd0bc029`.

<a id="q-0a0c784aaddf9c3c2c394c3c1e20e74f95e634721705e2428aa39fb817697c65"></a>

FACT: Q-0a0c784aaddf: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-07&endDate=2021-11-07); parameters=`{"endDate": "2021-11-07", "startDate": "2021-11-07"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:33.187330Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:33.548430Z"}]`. Manifest query ID `0a0c784aaddf9c3c2c394c3c1e20e74f95e634721705e2428aa39fb817697c65`.

<a id="q-d8f6a08d7b3bebf57250d924aa7acae245b76be15097e5bddbad841bad463529"></a>

FACT: Q-d8f6a08d7b3b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-12&endDate=2021-11-18); parameters=`{"endDate": "2021-11-18", "startDate": "2021-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:33.691849Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:34.048505Z"}]`. Manifest query ID `d8f6a08d7b3bebf57250d924aa7acae245b76be15097e5bddbad841bad463529`.

<a id="q-4cb4f4a24466b67192b5fa50139cfe5d199f4ffda26bc123e35a4f086ad56921"></a>

FACT: Q-4cb4f4a24466: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-19&endDate=2021-11-25); parameters=`{"endDate": "2021-11-25", "startDate": "2021-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:34.150410Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:34.548585Z"}]`. Manifest query ID `4cb4f4a24466b67192b5fa50139cfe5d199f4ffda26bc123e35a4f086ad56921`.

<a id="q-8c611c4ce7f0064434fa4b90b2089cb6ba8060680de1e7a77779454d330c5350"></a>

FACT: Q-8c611c4ce7f0: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-11-26&endDate=2021-12-02); parameters=`{"endDate": "2021-12-02", "startDate": "2021-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:34.657813Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:35.048662Z"}]`. Manifest query ID `8c611c4ce7f0064434fa4b90b2089cb6ba8060680de1e7a77779454d330c5350`.

<a id="q-72f1a021c25024c96826e2695b3cc8cebcd13de9983552ee510633420467dd54"></a>

FACT: Q-72f1a021c250: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-03&endDate=2021-12-09); parameters=`{"endDate": "2021-12-09", "startDate": "2021-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:35.172812Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:35.548737Z"}]`. Manifest query ID `72f1a021c25024c96826e2695b3cc8cebcd13de9983552ee510633420467dd54`.

<a id="q-7195337c606108ae447c379b8e538d7e05e83294a9b8381cffb8450ced9b0294"></a>

FACT: Q-7195337c6061: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-10&endDate=2021-12-16); parameters=`{"endDate": "2021-12-16", "startDate": "2021-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:35.661103Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:36.048829Z"}]`. Manifest query ID `7195337c606108ae447c379b8e538d7e05e83294a9b8381cffb8450ced9b0294`.

<a id="q-9f566c0058bae739d7925f22e8289f84631cea9867e5adb87305fbe9bc6dab99"></a>

FACT: Q-9f566c0058ba: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-17&endDate=2021-12-23); parameters=`{"endDate": "2021-12-23", "startDate": "2021-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:36.147970Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:36.548907Z"}]`. Manifest query ID `9f566c0058bae739d7925f22e8289f84631cea9867e5adb87305fbe9bc6dab99`.

<a id="q-31a281d6c157d362d720a39dc0eaf229551a09a57d4e50d642217290ac96ebf3"></a>

FACT: Q-31a281d6c157: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-24&endDate=2021-12-30); parameters=`{"endDate": "2021-12-30", "startDate": "2021-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:36.633508Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:37.048981Z"}]`. Manifest query ID `31a281d6c157d362d720a39dc0eaf229551a09a57d4e50d642217290ac96ebf3`.

<a id="q-6bb7e448dd5b3225e032c0827977f1bfe8168658c42b730bda2e8f8041d5b349"></a>

FACT: Q-6bb7e448dd5b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-31&endDate=2021-12-31); parameters=`{"endDate": "2021-12-31", "startDate": "2021-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:37.137345Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:37.549068Z"}]`. Manifest query ID `6bb7e448dd5b3225e032c0827977f1bfe8168658c42b730bda2e8f8041d5b349`.

<a id="q-38ecece1bab4bb3566abf28d50e619df205c8260e017c5e3d2db9d9285d864ff"></a>

FACT: Q-38ecece1bab4: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-01&endDate=2022-10-07); parameters=`{"endDate": "2022-10-07", "startDate": "2022-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:43.537434Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:43.537473Z"}]`. Manifest query ID `38ecece1bab4bb3566abf28d50e619df205c8260e017c5e3d2db9d9285d864ff`.

<a id="q-be7873ee63ca7e03f23f7ae8bd7c3214fb90e27b45882b8a7263f93b1aa40b0c"></a>

FACT: Q-be7873ee63ca: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-01&endDate=2022-10-04); parameters=`{"endDate": "2022-10-04", "startDate": "2022-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:43.694561Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:44.037554Z"}]`. Manifest query ID `be7873ee63ca7e03f23f7ae8bd7c3214fb90e27b45882b8a7263f93b1aa40b0c`.

<a id="q-f5f66747e96df5d486dbb33d774e3903fbc68609c337c55b3a76c38082fd2cd9"></a>

FACT: Q-f5f66747e96d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-05&endDate=2022-10-07); parameters=`{"endDate": "2022-10-07", "startDate": "2022-10-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:44.133236Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:44.537649Z"}]`. Manifest query ID `f5f66747e96df5d486dbb33d774e3903fbc68609c337c55b3a76c38082fd2cd9`.

<a id="q-995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526"></a>

FACT: Q-995fd6d73263: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-05&endDate=2022-10-06); parameters=`{"endDate": "2022-10-06", "startDate": "2022-10-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:44.690880Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:45.037721Z"}]`. Manifest query ID `995fd6d7326378c3c5e59865cf2f13264a66fbd8aee31e2d6a31a70178e35526`.

<a id="q-85e73088c1e7cf54a7ab429a8ad27c58785a0e366e66d3eeb794736040834097"></a>

FACT: Q-85e73088c1e7: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-07&endDate=2022-10-07); parameters=`{"endDate": "2022-10-07", "startDate": "2022-10-07"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:45.190436Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:45.537798Z"}]`. Manifest query ID `85e73088c1e7cf54a7ab429a8ad27c58785a0e366e66d3eeb794736040834097`.

<a id="q-72bb1eb2ea8dde63b15305a2ddbbc45eceb54c80e98ff579193b62b0d409d06a"></a>

FACT: Q-72bb1eb2ea8d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-08&endDate=2022-10-14); parameters=`{"endDate": "2022-10-14", "startDate": "2022-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:45.633812Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:46.037873Z"}]`. Manifest query ID `72bb1eb2ea8dde63b15305a2ddbbc45eceb54c80e98ff579193b62b0d409d06a`.

<a id="q-54cfd4c3968c02de0241ea8db0e056eb424b430e1da43db820006ff90dee4b04"></a>

FACT: Q-54cfd4c3968c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-15&endDate=2022-10-21); parameters=`{"endDate": "2022-10-21", "startDate": "2022-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:46.153151Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:46.537962Z"}]`. Manifest query ID `54cfd4c3968c02de0241ea8db0e056eb424b430e1da43db820006ff90dee4b04`.

<a id="q-103d9e875d68907fc97b3918fd3c293233636f355fdaf152592455443bfe7661"></a>

FACT: Q-103d9e875d68: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-22&endDate=2022-10-28); parameters=`{"endDate": "2022-10-28", "startDate": "2022-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:46.671157Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:47.038041Z"}]`. Manifest query ID `103d9e875d68907fc97b3918fd3c293233636f355fdaf152592455443bfe7661`.

<a id="q-c6f8da0d8dc9af9d7be8428a291e5513b9eba7bf6ca58af604d617147cbbd9fe"></a>

FACT: Q-c6f8da0d8dc9: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-10-29&endDate=2022-11-04); parameters=`{"endDate": "2022-11-04", "startDate": "2022-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:47.149321Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:47.538145Z"}]`. Manifest query ID `c6f8da0d8dc9af9d7be8428a291e5513b9eba7bf6ca58af604d617147cbbd9fe`.

<a id="q-8519761fbb3ccebad7beac8d83f54fd5797b1eba9539dce96e0f2d50d193cb5c"></a>

FACT: Q-8519761fbb3c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-05&endDate=2022-11-11); parameters=`{"endDate": "2022-11-11", "startDate": "2022-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:47.653239Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:48.038228Z"}]`. Manifest query ID `8519761fbb3ccebad7beac8d83f54fd5797b1eba9539dce96e0f2d50d193cb5c`.

<a id="q-1107719628a649bed19a3549396a3fe3ad40899091403064cfa63aa48971db9a"></a>

FACT: Q-1107719628a6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-05&endDate=2022-11-08); parameters=`{"endDate": "2022-11-08", "startDate": "2022-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:48.257944Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:48.538302Z"}]`. Manifest query ID `1107719628a649bed19a3549396a3fe3ad40899091403064cfa63aa48971db9a`.

<a id="q-c28076a25efaa87181960ff074c2d10314c9b6fcc845f6976d7d1b0cf311ebbc"></a>

FACT: Q-c28076a25efa: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-09&endDate=2022-11-11); parameters=`{"endDate": "2022-11-11", "startDate": "2022-11-09"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:48.682561Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:49.038386Z"}]`. Manifest query ID `c28076a25efaa87181960ff074c2d10314c9b6fcc845f6976d7d1b0cf311ebbc`.

<a id="q-879347af57ab12cbf87f566053e773c5917cb6de06e5bd185923b00f71eb0dcb"></a>

FACT: Q-879347af57ab: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-09&endDate=2022-11-10); parameters=`{"endDate": "2022-11-10", "startDate": "2022-11-09"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:49.221752Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:49.538459Z"}]`. Manifest query ID `879347af57ab12cbf87f566053e773c5917cb6de06e5bd185923b00f71eb0dcb`.

<a id="q-6e8c4cc31f2b4cf6b104676b0a5c2bfad25e9146518c7cc54ace9e3960713a37"></a>

FACT: Q-6e8c4cc31f2b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-09&endDate=2022-11-09); parameters=`{"endDate": "2022-11-09", "startDate": "2022-11-09"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:49.728443Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:50.038535Z"}]`. Manifest query ID `6e8c4cc31f2b4cf6b104676b0a5c2bfad25e9146518c7cc54ace9e3960713a37`.

<a id="q-549254210526f21161cb51abcb128a28e2b9dec26b1a934696a4c360881c5817"></a>

FACT: Q-549254210526: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-10&endDate=2022-11-10); parameters=`{"endDate": "2022-11-10", "startDate": "2022-11-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:50.134679Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:50.538607Z"}]`. Manifest query ID `549254210526f21161cb51abcb128a28e2b9dec26b1a934696a4c360881c5817`.

<a id="q-8077ce89df627ae4311a84ce045ec8269d54ef3d0e789449a5a17668f3ccb894"></a>

FACT: Q-8077ce89df62: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-12&endDate=2022-11-18); parameters=`{"endDate": "2022-11-18", "startDate": "2022-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:50.711814Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:51.038687Z"}]`. Manifest query ID `8077ce89df627ae4311a84ce045ec8269d54ef3d0e789449a5a17668f3ccb894`.

<a id="q-f02106d1f842c7b5d6de1421a8ef3bfac7c50dd8c3db0f6d49f6964a0b746d38"></a>

FACT: Q-f02106d1f842: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-19&endDate=2022-11-25); parameters=`{"endDate": "2022-11-25", "startDate": "2022-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:51.171083Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:51.538760Z"}]`. Manifest query ID `f02106d1f842c7b5d6de1421a8ef3bfac7c50dd8c3db0f6d49f6964a0b746d38`.

<a id="q-e93b9edc20f556fc51128ebd17fcad94eb0ddea72eb9fb48978c6f7a90bfee5e"></a>

FACT: Q-e93b9edc20f5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-11-26&endDate=2022-12-02); parameters=`{"endDate": "2022-12-02", "startDate": "2022-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:51.645399Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:52.038842Z"}]`. Manifest query ID `e93b9edc20f556fc51128ebd17fcad94eb0ddea72eb9fb48978c6f7a90bfee5e`.

<a id="q-646b38d8ded695a10bff6370d33cd4a392c5b0ae99dfdf8c1c561912aed4c0e1"></a>

FACT: Q-646b38d8ded6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-03&endDate=2022-12-09); parameters=`{"endDate": "2022-12-09", "startDate": "2022-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:52.137163Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:52.538914Z"}]`. Manifest query ID `646b38d8ded695a10bff6370d33cd4a392c5b0ae99dfdf8c1c561912aed4c0e1`.

<a id="q-5bcab56126c024f4fa9ba66235ccd5ba4527f9a38523cdbf678482788f9eead8"></a>

FACT: Q-5bcab56126c0: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-10&endDate=2022-12-16); parameters=`{"endDate": "2022-12-16", "startDate": "2022-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:52.650784Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:53.038989Z"}]`. Manifest query ID `5bcab56126c024f4fa9ba66235ccd5ba4527f9a38523cdbf678482788f9eead8`.

<a id="q-acacf5d43d22f9d89c9f2ea101ba340c858674ff80ee1524b3ba35e58d9e4f0e"></a>

FACT: Q-acacf5d43d22: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-17&endDate=2022-12-23); parameters=`{"endDate": "2022-12-23", "startDate": "2022-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:53.168420Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:53.539058Z"}]`. Manifest query ID `acacf5d43d22f9d89c9f2ea101ba340c858674ff80ee1524b3ba35e58d9e4f0e`.

<a id="q-04ff6b4f9ab81d16b0843993d30a2814042e8bca9936077399aabdc7ca3fe91c"></a>

FACT: Q-04ff6b4f9ab8: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-24&endDate=2022-12-30); parameters=`{"endDate": "2022-12-30", "startDate": "2022-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:53.650565Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:54.039173Z"}]`. Manifest query ID `04ff6b4f9ab81d16b0843993d30a2814042e8bca9936077399aabdc7ca3fe91c`.

<a id="q-a5a62f9f6fec77900b3f6566dfec38bb1719b0b47d0aceaa58723321385b7b4a"></a>

FACT: Q-a5a62f9f6fec: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-31&endDate=2022-12-31); parameters=`{"endDate": "2022-12-31", "startDate": "2022-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:34:54.125335Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:34:54.539250Z"}]`. Manifest query ID `a5a62f9f6fec77900b3f6566dfec38bb1719b0b47d0aceaa58723321385b7b4a`.

<a id="q-bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281"></a>

FACT: Q-bb4c91ea405d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-01&endDate=2023-10-07); parameters=`{"endDate": "2023-10-07", "startDate": "2023-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:02.424582Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:02.424615Z"}]`. Manifest query ID `bb4c91ea405d95678a71220fc9869a5e21adad60cec4c35ccb00d9ff0d499281`.

<a id="q-39846f85a292b63175c219f353acf6ca22040d8edf0528788834cc21d3a0bc5f"></a>

FACT: Q-39846f85a292: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-08&endDate=2023-10-14); parameters=`{"endDate": "2023-10-14", "startDate": "2023-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:02.559331Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:02.924694Z"}]`. Manifest query ID `39846f85a292b63175c219f353acf6ca22040d8edf0528788834cc21d3a0bc5f`.

<a id="q-0dfdd7c7a98862a7b8a3e60b32962b2ec448d45dc686b28e5d691ab282a8cde8"></a>

FACT: Q-0dfdd7c7a988: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-15&endDate=2023-10-21); parameters=`{"endDate": "2023-10-21", "startDate": "2023-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:03.040684Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:03.424772Z"}]`. Manifest query ID `0dfdd7c7a98862a7b8a3e60b32962b2ec448d45dc686b28e5d691ab282a8cde8`.

<a id="q-f2747a84a9240cfedc689dc4f034751c7659238d7fa9c11e6ad19d475e574455"></a>

FACT: Q-f2747a84a924: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-22&endDate=2023-10-28); parameters=`{"endDate": "2023-10-28", "startDate": "2023-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:03.543669Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:03.924860Z"}]`. Manifest query ID `f2747a84a9240cfedc689dc4f034751c7659238d7fa9c11e6ad19d475e574455`.

<a id="q-6e1aabb3191240cf12cb39e991a408d10b7080b408883d34c13fdd55b3d8d15d"></a>

FACT: Q-6e1aabb31912: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-29&endDate=2023-11-04); parameters=`{"endDate": "2023-11-04", "startDate": "2023-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:04.020116Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:04.424935Z"}]`. Manifest query ID `6e1aabb3191240cf12cb39e991a408d10b7080b408883d34c13fdd55b3d8d15d`.

<a id="q-14771f43df16fd7abe0bfe8c6240c0e84c1fbde3dd84a5bb9f3c43a92b218dd9"></a>

FACT: Q-14771f43df16: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-10-29&endDate=2023-11-01); parameters=`{"endDate": "2023-11-01", "startDate": "2023-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:04.586634Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:04.925024Z"}]`. Manifest query ID `14771f43df16fd7abe0bfe8c6240c0e84c1fbde3dd84a5bb9f3c43a92b218dd9`.

<a id="q-5e6ff3057aa6b765c504a04ccdfb5da11d029a6ca81e25b6ef331b05987e8ae4"></a>

FACT: Q-5e6ff3057aa6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-02&endDate=2023-11-04); parameters=`{"endDate": "2023-11-04", "startDate": "2023-11-02"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:05.011818Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:05.425099Z"}]`. Manifest query ID `5e6ff3057aa6b765c504a04ccdfb5da11d029a6ca81e25b6ef331b05987e8ae4`.

<a id="q-263d31f6722e5906565fde37aea2eacf0c99d43cf055f07a2b8d9cbdab69da1b"></a>

FACT: Q-263d31f6722e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-05&endDate=2023-11-11); parameters=`{"endDate": "2023-11-11", "startDate": "2023-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:05.558143Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:05.925177Z"}]`. Manifest query ID `263d31f6722e5906565fde37aea2eacf0c99d43cf055f07a2b8d9cbdab69da1b`.

<a id="q-ac8b9f09ddc5b4f7fa7906b4c32c34f8a428a94af359fa7593b2d75e0c615b6e"></a>

FACT: Q-ac8b9f09ddc5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-05&endDate=2023-11-08); parameters=`{"endDate": "2023-11-08", "startDate": "2023-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:06.146821Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:06.425254Z"}]`. Manifest query ID `ac8b9f09ddc5b4f7fa7906b4c32c34f8a428a94af359fa7593b2d75e0c615b6e`.

<a id="q-b9c73297db2ce052f9a5a7dbf87f63007b0f5ee02d482e526bb8b3be01c000db"></a>

FACT: Q-b9c73297db2c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-05&endDate=2023-11-06); parameters=`{"endDate": "2023-11-06", "startDate": "2023-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:06.646504Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:06.925364Z"}]`. Manifest query ID `b9c73297db2ce052f9a5a7dbf87f63007b0f5ee02d482e526bb8b3be01c000db`.

<a id="q-6d043e1d6f84b223323e0e3da86ff6cd914b9c75fd6080ebe821775f35d7e364"></a>

FACT: Q-6d043e1d6f84: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-05&endDate=2023-11-05); parameters=`{"endDate": "2023-11-05", "startDate": "2023-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:07.114530Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:07.425442Z"}]`. Manifest query ID `6d043e1d6f84b223323e0e3da86ff6cd914b9c75fd6080ebe821775f35d7e364`.

<a id="q-0aabf2973ba6db8c3fc8f6d032c939dba6f7b084da23a39ab00cc5c067577d59"></a>

FACT: Q-0aabf2973ba6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-06&endDate=2023-11-06); parameters=`{"endDate": "2023-11-06", "startDate": "2023-11-06"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:07.504521Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:07.925516Z"}]`. Manifest query ID `0aabf2973ba6db8c3fc8f6d032c939dba6f7b084da23a39ab00cc5c067577d59`.

<a id="q-61f74abb33c74f178765ebc706f1fcdbd35a5cde11753ccf4c98c82548d5cffe"></a>

FACT: Q-61f74abb33c7: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-12&endDate=2023-11-18); parameters=`{"endDate": "2023-11-18", "startDate": "2023-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:08.117540Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:08.425610Z"}]`. Manifest query ID `61f74abb33c74f178765ebc706f1fcdbd35a5cde11753ccf4c98c82548d5cffe`.

<a id="q-91a88687937d0e01cee428a1396175bc9922cf06604d122b6ec06d28b70ec08f"></a>

FACT: Q-91a88687937d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-19&endDate=2023-11-25); parameters=`{"endDate": "2023-11-25", "startDate": "2023-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:08.561295Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:08.925727Z"}]`. Manifest query ID `91a88687937d0e01cee428a1396175bc9922cf06604d122b6ec06d28b70ec08f`.

<a id="q-2226359382bb5f93a59d68e1940fda58ab6fd77eb878eb88a1013aa0512c523a"></a>

FACT: Q-2226359382bb: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-11-26&endDate=2023-12-02); parameters=`{"endDate": "2023-12-02", "startDate": "2023-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:09.035417Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:09.425812Z"}]`. Manifest query ID `2226359382bb5f93a59d68e1940fda58ab6fd77eb878eb88a1013aa0512c523a`.

<a id="q-d1cbe8f013010981a5938e47e3b3b2ce129501e7e4fa4b49e3dc85676131e989"></a>

FACT: Q-d1cbe8f01301: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-03&endDate=2023-12-09); parameters=`{"endDate": "2023-12-09", "startDate": "2023-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:09.533412Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:09.925916Z"}]`. Manifest query ID `d1cbe8f013010981a5938e47e3b3b2ce129501e7e4fa4b49e3dc85676131e989`.

<a id="q-fc01607c15824dd2d4d6fff869f32bb72f94625a090850b527d14f3e14f7c04a"></a>

FACT: Q-fc01607c1582: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-10&endDate=2023-12-16); parameters=`{"endDate": "2023-12-16", "startDate": "2023-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:10.042988Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:10.425988Z"}]`. Manifest query ID `fc01607c15824dd2d4d6fff869f32bb72f94625a090850b527d14f3e14f7c04a`.

<a id="q-817a54a63640933e98d3912e916e23352dcf656a998e39b5050f361f677b72a2"></a>

FACT: Q-817a54a63640: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-17&endDate=2023-12-23); parameters=`{"endDate": "2023-12-23", "startDate": "2023-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:10.527809Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:10.926037Z"}]`. Manifest query ID `817a54a63640933e98d3912e916e23352dcf656a998e39b5050f361f677b72a2`.

<a id="q-23627979a224a34edd659e971da9e0896e828b7a9929eacf752557a613300190"></a>

FACT: Q-23627979a224: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-24&endDate=2023-12-30); parameters=`{"endDate": "2023-12-30", "startDate": "2023-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:11.046542Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:11.426110Z"}]`. Manifest query ID `23627979a224a34edd659e971da9e0896e828b7a9929eacf752557a613300190`.

<a id="q-34e4ae8d9a7e875b02e7acb5beda699d5735a674746ccbe53094f55a66fb6241"></a>

FACT: Q-34e4ae8d9a7e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-31&endDate=2023-12-31); parameters=`{"endDate": "2023-12-31", "startDate": "2023-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:11.533709Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:11.926197Z"}]`. Manifest query ID `34e4ae8d9a7e875b02e7acb5beda699d5735a674746ccbe53094f55a66fb6241`.

<a id="q-59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4"></a>

FACT: Q-59a34d51aafb: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-01&endDate=2024-10-07); parameters=`{"endDate": "2024-10-07", "startDate": "2024-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:16.604980Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:16.605030Z"}]`. Manifest query ID `59a34d51aafb7a100068c649b941add448070429d2514dccf89a80bb32036dc4`.

<a id="q-bfcdfef73935567d9c37db9eb966780df55e65678a101bd57b189720c5ed49b6"></a>

FACT: Q-bfcdfef73935: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-08&endDate=2024-10-14); parameters=`{"endDate": "2024-10-14", "startDate": "2024-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:16.736197Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:17.105112Z"}]`. Manifest query ID `bfcdfef73935567d9c37db9eb966780df55e65678a101bd57b189720c5ed49b6`.

<a id="q-a99d8651626eb79ec39fe6f15ea85332894604470234455d5a1eaef8790c501e"></a>

FACT: Q-a99d8651626e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-15&endDate=2024-10-21); parameters=`{"endDate": "2024-10-21", "startDate": "2024-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:17.216489Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:17.605200Z"}]`. Manifest query ID `a99d8651626eb79ec39fe6f15ea85332894604470234455d5a1eaef8790c501e`.

<a id="q-9ef9946519d16d2691c912e283912d9083cde0cabc66319cf94ed1728e275921"></a>

FACT: Q-9ef9946519d1: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-22&endDate=2024-10-28); parameters=`{"endDate": "2024-10-28", "startDate": "2024-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:17.710524Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:18.105281Z"}]`. Manifest query ID `9ef9946519d16d2691c912e283912d9083cde0cabc66319cf94ed1728e275921`.

<a id="q-431b1bd6a5e1bb04c1b7ff8465c76032abb88a6f41adbadd866ffb475eca722d"></a>

FACT: Q-431b1bd6a5e1: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-29&endDate=2024-11-04); parameters=`{"endDate": "2024-11-04", "startDate": "2024-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:18.203723Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:18.605365Z"}]`. Manifest query ID `431b1bd6a5e1bb04c1b7ff8465c76032abb88a6f41adbadd866ffb475eca722d`.

<a id="q-68bdca9053a68e1386208afec71e9d567114c7ad82f8d2a3878cb535cf69199e"></a>

FACT: Q-68bdca9053a6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-10-29&endDate=2024-11-01); parameters=`{"endDate": "2024-11-01", "startDate": "2024-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:18.841320Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:19.105438Z"}]`. Manifest query ID `68bdca9053a68e1386208afec71e9d567114c7ad82f8d2a3878cb535cf69199e`.

<a id="q-fb5e78d3eefbb43492957291dd56e0cd2db4da75c02e35f00257c223d5fb157e"></a>

FACT: Q-fb5e78d3eefb: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-02&endDate=2024-11-04); parameters=`{"endDate": "2024-11-04", "startDate": "2024-11-02"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:19.224733Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:19.605525Z"}]`. Manifest query ID `fb5e78d3eefbb43492957291dd56e0cd2db4da75c02e35f00257c223d5fb157e`.

<a id="q-6589048a0d21505fb218f44f6cc503cdaa846cd94390b53d66b2bde2aa3eb6f1"></a>

FACT: Q-6589048a0d21: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-02&endDate=2024-11-03); parameters=`{"endDate": "2024-11-03", "startDate": "2024-11-02"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:19.804514Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:20.105595Z"}]`. Manifest query ID `6589048a0d21505fb218f44f6cc503cdaa846cd94390b53d66b2bde2aa3eb6f1`.

<a id="q-637384fcb33ca7506d46a6d73f95ffb015ae72f74bb0c74259fbadb6322fbc82"></a>

FACT: Q-637384fcb33c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-04&endDate=2024-11-04); parameters=`{"endDate": "2024-11-04", "startDate": "2024-11-04"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:20.219784Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:20.605681Z"}]`. Manifest query ID `637384fcb33ca7506d46a6d73f95ffb015ae72f74bb0c74259fbadb6322fbc82`.

<a id="q-c8159d2fe7ea835df2f6d35ce1cae9901047ea905d483dd34cf94b87c32b1691"></a>

FACT: Q-c8159d2fe7ea: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-05&endDate=2024-11-11); parameters=`{"endDate": "2024-11-11", "startDate": "2024-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:20.752808Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:21.105982Z"}]`. Manifest query ID `c8159d2fe7ea835df2f6d35ce1cae9901047ea905d483dd34cf94b87c32b1691`.

<a id="q-681cc2749c078a54e8510fdb5ed55fb5a1c2a8c320571393dc4e6fdf9e9f199e"></a>

FACT: Q-681cc2749c07: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-12&endDate=2024-11-18); parameters=`{"endDate": "2024-11-18", "startDate": "2024-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:21.235623Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:21.606041Z"}]`. Manifest query ID `681cc2749c078a54e8510fdb5ed55fb5a1c2a8c320571393dc4e6fdf9e9f199e`.

<a id="q-9d0b4c235d457a8570e4fbbda2be47158207335cc72a87561ca973398850e063"></a>

FACT: Q-9d0b4c235d45: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-19&endDate=2024-11-25); parameters=`{"endDate": "2024-11-25", "startDate": "2024-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:21.708445Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:22.106115Z"}]`. Manifest query ID `9d0b4c235d457a8570e4fbbda2be47158207335cc72a87561ca973398850e063`.

<a id="q-f953f8cb15689162f82ef2ee38cf83c17541f528b509474f8965f0c435d77087"></a>

FACT: Q-f953f8cb1568: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-11-26&endDate=2024-12-02); parameters=`{"endDate": "2024-12-02", "startDate": "2024-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:22.225360Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:22.606192Z"}]`. Manifest query ID `f953f8cb15689162f82ef2ee38cf83c17541f528b509474f8965f0c435d77087`.

<a id="q-1ea6aeb6725b611dac7b8b6e8e5b5d9741371b0be674bfab95aa0eae9821d4a7"></a>

FACT: Q-1ea6aeb6725b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-03&endDate=2024-12-09); parameters=`{"endDate": "2024-12-09", "startDate": "2024-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:22.707933Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:23.106266Z"}]`. Manifest query ID `1ea6aeb6725b611dac7b8b6e8e5b5d9741371b0be674bfab95aa0eae9821d4a7`.

<a id="q-3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005"></a>

FACT: Q-3ec85a62dfc5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-10&endDate=2024-12-16); parameters=`{"endDate": "2024-12-16", "startDate": "2024-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:23.296336Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:23.606386Z"}]`. Manifest query ID `3ec85a62dfc58390f61db193751ea1fe7587ad37b0594ce2e83b1c0e3bbbe005`.

<a id="q-a9a03d66afd20c182cad4c118f8751f194aa209d85d4a7685e02444091c5686b"></a>

FACT: Q-a9a03d66afd2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-17&endDate=2024-12-23); parameters=`{"endDate": "2024-12-23", "startDate": "2024-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:23.738808Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:24.106457Z"}]`. Manifest query ID `a9a03d66afd20c182cad4c118f8751f194aa209d85d4a7685e02444091c5686b`.

<a id="q-32fbb85bda2116656ea1424c7ab86905fd1d8d70125b62b5e49283e860135d93"></a>

FACT: Q-32fbb85bda21: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-24&endDate=2024-12-30); parameters=`{"endDate": "2024-12-30", "startDate": "2024-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:24.208874Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:24.606653Z"}]`. Manifest query ID `32fbb85bda2116656ea1424c7ab86905fd1d8d70125b62b5e49283e860135d93`.

<a id="q-1c2ee5a90e5d8ace4c8665a5eb69d36b34d23caa047890152e3e3d545c7145d9"></a>

FACT: Q-1c2ee5a90e5d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-31&endDate=2024-12-31); parameters=`{"endDate": "2024-12-31", "startDate": "2024-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:24.688917Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:25.106724Z"}]`. Manifest query ID `1c2ee5a90e5d8ace4c8665a5eb69d36b34d23caa047890152e3e3d545c7145d9`.

<a id="q-a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a"></a>

FACT: Q-a691748e5f44: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-01&endDate=2025-10-07); parameters=`{"endDate": "2025-10-07", "startDate": "2025-10-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:32.671695Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:32.671725Z"}]`. Manifest query ID `a691748e5f44bb0c92a842d49a2cf3bb5065a4754921f92209a60fceb7aa908a`.

<a id="q-80eeb9d2b0740a9af4844b5b5341e9010f860d635911285c75e0e9aefb146066"></a>

FACT: Q-80eeb9d2b074: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-08&endDate=2025-10-14); parameters=`{"endDate": "2025-10-14", "startDate": "2025-10-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:32.860543Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:33.171816Z"}]`. Manifest query ID `80eeb9d2b0740a9af4844b5b5341e9010f860d635911285c75e0e9aefb146066`.

<a id="q-b9081186ce46bc13b7d75d03caa88a108d1eca8b73a9625ccf159d40c54e4e15"></a>

FACT: Q-b9081186ce46: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-15&endDate=2025-10-21); parameters=`{"endDate": "2025-10-21", "startDate": "2025-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:33.285974Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:33.671910Z"}]`. Manifest query ID `b9081186ce46bc13b7d75d03caa88a108d1eca8b73a9625ccf159d40c54e4e15`.

<a id="q-8166064bdb7b9628eb2e31ab48718e7fe01525454ef5f7642ff5c360aab9a86b"></a>

FACT: Q-8166064bdb7b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-15&endDate=2025-10-18); parameters=`{"endDate": "2025-10-18", "startDate": "2025-10-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:33.833616Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:34.171999Z"}]`. Manifest query ID `8166064bdb7b9628eb2e31ab48718e7fe01525454ef5f7642ff5c360aab9a86b`.

<a id="q-9f3e47eef4ad0ebdf25495db690b3e0597500fa96388c88fb96986a1227ae130"></a>

FACT: Q-9f3e47eef4ad: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-19&endDate=2025-10-21); parameters=`{"endDate": "2025-10-21", "startDate": "2025-10-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:34.314730Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:34.672055Z"}]`. Manifest query ID `9f3e47eef4ad0ebdf25495db690b3e0597500fa96388c88fb96986a1227ae130`.

<a id="q-d8747801b40cd68ba12c2c6278e6147b721280964a4b484d9a38f4943ba53f92"></a>

FACT: Q-d8747801b40c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-22&endDate=2025-10-28); parameters=`{"endDate": "2025-10-28", "startDate": "2025-10-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:34.766374Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:35.172141Z"}]`. Manifest query ID `d8747801b40cd68ba12c2c6278e6147b721280964a4b484d9a38f4943ba53f92`.

<a id="q-5d2e5cd378b34806ff37b314ecc53a2cd9e9a386198508e7b7e8711c376ce315"></a>

FACT: Q-5d2e5cd378b3: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-10-29&endDate=2025-11-04); parameters=`{"endDate": "2025-11-04", "startDate": "2025-10-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:35.284627Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:35.672239Z"}]`. Manifest query ID `5d2e5cd378b34806ff37b314ecc53a2cd9e9a386198508e7b7e8711c376ce315`.

<a id="q-4b9361fd4b1675a4efc0bacd6adb7b0e5f3ef46161ede0ac2fc97c7c4196fd56"></a>

FACT: Q-4b9361fd4b16: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-05&endDate=2025-11-11); parameters=`{"endDate": "2025-11-11", "startDate": "2025-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:35.786671Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:36.172337Z"}]`. Manifest query ID `4b9361fd4b1675a4efc0bacd6adb7b0e5f3ef46161ede0ac2fc97c7c4196fd56`.

<a id="q-d2f7101b1aaf87f3972e7fadb38290d0f53b0d315340d2b5f8195e19a74737b6"></a>

FACT: Q-d2f7101b1aaf: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-05&endDate=2025-11-08); parameters=`{"endDate": "2025-11-08", "startDate": "2025-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:36.392069Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:36.672430Z"}]`. Manifest query ID `d2f7101b1aaf87f3972e7fadb38290d0f53b0d315340d2b5f8195e19a74737b6`.

<a id="q-f06e99803daef30ffc886f130251c03c26839880b968535e6848bdd1c0dd809e"></a>

FACT: Q-f06e99803dae: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-05&endDate=2025-11-06); parameters=`{"endDate": "2025-11-06", "startDate": "2025-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:36.956925Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:37.172527Z"}]`. Manifest query ID `f06e99803daef30ffc886f130251c03c26839880b968535e6848bdd1c0dd809e`.

<a id="q-2a3137531c16d43dde4935133ac1a741d748d837c971154da4c2a60b6a1a442e"></a>

FACT: Q-2a3137531c16: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-05&endDate=2025-11-05); parameters=`{"endDate": "2025-11-05", "startDate": "2025-11-05"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:37.414219Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:37.672608Z"}]`. Manifest query ID `2a3137531c16d43dde4935133ac1a741d748d837c971154da4c2a60b6a1a442e`.

<a id="q-4ed863d1969ae369a26cba61d36877579265e00590c5208aa9a332ee93c825ab"></a>

FACT: Q-4ed863d1969a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-06&endDate=2025-11-06); parameters=`{"endDate": "2025-11-06", "startDate": "2025-11-06"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:37.751292Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:38.172692Z"}]`. Manifest query ID `4ed863d1969ae369a26cba61d36877579265e00590c5208aa9a332ee93c825ab`.

<a id="q-3fdb4502639fdad581001be0afaddda926a4bd5f62984cefb0181848f0064694"></a>

FACT: Q-3fdb4502639f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-12&endDate=2025-11-18); parameters=`{"endDate": "2025-11-18", "startDate": "2025-11-12"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:38.443868Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:38.672808Z"}]`. Manifest query ID `3fdb4502639fdad581001be0afaddda926a4bd5f62984cefb0181848f0064694`.

<a id="q-ea68a7549a9a4fdf726f401969fa740c8000b963b4e1af5bc49dd92eaa1ec9a1"></a>

FACT: Q-ea68a7549a9a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-19&endDate=2025-11-25); parameters=`{"endDate": "2025-11-25", "startDate": "2025-11-19"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:38.801477Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:39.172899Z"}]`. Manifest query ID `ea68a7549a9a4fdf726f401969fa740c8000b963b4e1af5bc49dd92eaa1ec9a1`.

<a id="q-d30f582e3b5d75d9078172e70e45134a3cf9881c59d5f3e252dc4c9e49b2bacd"></a>

FACT: Q-d30f582e3b5d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-11-26&endDate=2025-12-02); parameters=`{"endDate": "2025-12-02", "startDate": "2025-11-26"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:39.278963Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:39.672990Z"}]`. Manifest query ID `d30f582e3b5d75d9078172e70e45134a3cf9881c59d5f3e252dc4c9e49b2bacd`.

<a id="q-4de7d93cb68752598a6d76655be814a92cf1325311d65685e3f0a72492237ee4"></a>

FACT: Q-4de7d93cb687: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-03&endDate=2025-12-09); parameters=`{"endDate": "2025-12-09", "startDate": "2025-12-03"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:39.776998Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:40.173059Z"}]`. Manifest query ID `4de7d93cb68752598a6d76655be814a92cf1325311d65685e3f0a72492237ee4`.

<a id="q-85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162"></a>

FACT: Q-85f4fc823916: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-10&endDate=2025-12-16); parameters=`{"endDate": "2025-12-16", "startDate": "2025-12-10"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:40.274975Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:40.673163Z"}]`. Manifest query ID `85f4fc823916cd9b466f5899e7897a35abf573f4d2e55b9f90064b4d6a276162`.

<a id="q-f0002ef24bf2472b2f58f86ce53951983191d2b528cea6ed8534200bc233b233"></a>

FACT: Q-f0002ef24bf2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-17&endDate=2025-12-23); parameters=`{"endDate": "2025-12-23", "startDate": "2025-12-17"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:40.824884Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:41.173261Z"}]`. Manifest query ID `f0002ef24bf2472b2f58f86ce53951983191d2b528cea6ed8534200bc233b233`.

<a id="q-d7aa234b0bf4f7c99babd021778b5e375686dd521aed4cfda3848ff7166a6392"></a>

FACT: Q-d7aa234b0bf4: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-24&endDate=2025-12-30); parameters=`{"endDate": "2025-12-30", "startDate": "2025-12-24"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:41.299767Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:41.673362Z"}]`. Manifest query ID `d7aa234b0bf4f7c99babd021778b5e375686dd521aed4cfda3848ff7166a6392`.

<a id="q-b292ac78cfec5bcb34d638661860a060e0a5c72b718f0c3c79faea81fffe75f9"></a>

FACT: Q-b292ac78cfec: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-31&endDate=2025-12-31); parameters=`{"endDate": "2025-12-31", "startDate": "2025-12-31"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:41.773174Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:42.173454Z"}]`. Manifest query ID `b292ac78cfec5bcb34d638661860a060e0a5c72b718f0c3c79faea81fffe75f9`.

<a id="q-d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b"></a>

FACT: Q-d9c8e40ee22d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2015-12-01&endDate=2015-12-07); parameters=`{"endDate": "2015-12-07", "startDate": "2015-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:46.583842Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:46.583876Z"}]`. Manifest query ID `d9c8e40ee22d2d4507774e104cd20e15112a4284ca31395cec32540966a6d06b`.

<a id="q-233d80c8096f1f7ef00061c24a09259889c2e2d115436ee935ca298126aee34c"></a>

FACT: Q-233d80c8096f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2015-12-08&endDate=2015-12-14); parameters=`{"endDate": "2015-12-14", "startDate": "2015-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:46.677408Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:47.083954Z"}]`. Manifest query ID `233d80c8096f1f7ef00061c24a09259889c2e2d115436ee935ca298126aee34c`.

<a id="q-19e72096bdc0bede16287aa14b1b1d6ac6dc1bd61adaeb6645bdde0e403b686d"></a>

FACT: Q-19e72096bdc0: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2015-12-15&endDate=2015-12-21); parameters=`{"endDate": "2015-12-21", "startDate": "2015-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:47.206208Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:47.584047Z"}]`. Manifest query ID `19e72096bdc0bede16287aa14b1b1d6ac6dc1bd61adaeb6645bdde0e403b686d`.

<a id="q-43e9763d2d3f89e3f3fbfe9b517cdadf2a577cc09afe70cdadb0f0a58d3dd8dd"></a>

FACT: Q-43e9763d2d3f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2015-12-22&endDate=2015-12-28); parameters=`{"endDate": "2015-12-28", "startDate": "2015-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:47.726674Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:48.084118Z"}]`. Manifest query ID `43e9763d2d3f89e3f3fbfe9b517cdadf2a577cc09afe70cdadb0f0a58d3dd8dd`.

<a id="q-5283c1735911819cc64fa9125120c494ac86befd74fdd54fe39542932b0f1ecc"></a>

FACT: Q-5283c1735911: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2015-12-29&endDate=2015-12-31); parameters=`{"endDate": "2015-12-31", "startDate": "2015-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:48.264993Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:48.584205Z"}]`. Manifest query ID `5283c1735911819cc64fa9125120c494ac86befd74fdd54fe39542932b0f1ecc`.

<a id="q-0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3"></a>

FACT: Q-0a2b75bb513f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2016-12-01&endDate=2016-12-07); parameters=`{"endDate": "2016-12-07", "startDate": "2016-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:48.976565Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:49.084274Z"}]`. Manifest query ID `0a2b75bb513fb7613aa1222e0ae2068d92f025988d2523eaddba8e29b60baba3`.

<a id="q-c390ddd9fa8850b2b4f5169fa77734af1858d07ba2f31be2e05a8026974fe5dd"></a>

FACT: Q-c390ddd9fa88: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2016-12-08&endDate=2016-12-14); parameters=`{"endDate": "2016-12-14", "startDate": "2016-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:49.179568Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:49.584353Z"}]`. Manifest query ID `c390ddd9fa8850b2b4f5169fa77734af1858d07ba2f31be2e05a8026974fe5dd`.

<a id="q-3d232100e262f518c01cba3ce51f8b50363b66d801d582141396b1f9f57bfc06"></a>

FACT: Q-3d232100e262: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2016-12-15&endDate=2016-12-21); parameters=`{"endDate": "2016-12-21", "startDate": "2016-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:49.690621Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:50.084431Z"}]`. Manifest query ID `3d232100e262f518c01cba3ce51f8b50363b66d801d582141396b1f9f57bfc06`.

<a id="q-f4871493ed80a598a5cb41672d1f73f4565bdb0ea257e74eb2035919afca37a2"></a>

FACT: Q-f4871493ed80: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2016-12-22&endDate=2016-12-28); parameters=`{"endDate": "2016-12-28", "startDate": "2016-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:50.183204Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:50.584506Z"}]`. Manifest query ID `f4871493ed80a598a5cb41672d1f73f4565bdb0ea257e74eb2035919afca37a2`.

<a id="q-c3d5e6f973e753ab956046f114b0de198208929d7e70f23ba2382ab081773653"></a>

FACT: Q-c3d5e6f973e7: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2016-12-29&endDate=2016-12-31); parameters=`{"endDate": "2016-12-31", "startDate": "2016-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:50.745427Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:51.084576Z"}]`. Manifest query ID `c3d5e6f973e753ab956046f114b0de198208929d7e70f23ba2382ab081773653`.

<a id="q-94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993"></a>

FACT: Q-94ea0454e66f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2017-12-01&endDate=2017-12-07); parameters=`{"endDate": "2017-12-07", "startDate": "2017-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:51.301120Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:51.584663Z"}]`. Manifest query ID `94ea0454e66f749777d4bdbb11f809f7dd899d070d3fbbc9aab0b12d634a2993`.

<a id="q-4e5dc646ff22f2d66c270f49bcbb62a0f2d5efe14fde796499e7e9cd9683a16d"></a>

FACT: Q-4e5dc646ff22: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2017-12-08&endDate=2017-12-14); parameters=`{"endDate": "2017-12-14", "startDate": "2017-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:51.681466Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:52.084734Z"}]`. Manifest query ID `4e5dc646ff22f2d66c270f49bcbb62a0f2d5efe14fde796499e7e9cd9683a16d`.

<a id="q-2e696bb0604b14d5cc0d616c041d88e808c6d3e422d8851dd31367745c42786a"></a>

FACT: Q-2e696bb0604b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2017-12-15&endDate=2017-12-21); parameters=`{"endDate": "2017-12-21", "startDate": "2017-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:52.186934Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:52.584806Z"}]`. Manifest query ID `2e696bb0604b14d5cc0d616c041d88e808c6d3e422d8851dd31367745c42786a`.

<a id="q-14ac1c3b2718a561b559acdb839a641418de2b10c6df2f8b626d8f1402105d8a"></a>

FACT: Q-14ac1c3b2718: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2017-12-22&endDate=2017-12-28); parameters=`{"endDate": "2017-12-28", "startDate": "2017-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:52.690217Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:53.084884Z"}]`. Manifest query ID `14ac1c3b2718a561b559acdb839a641418de2b10c6df2f8b626d8f1402105d8a`.

<a id="q-8814b82fd0b2f67c21f783a9f6b27b6eec037788220f4ef6bd7efb984bb2acb9"></a>

FACT: Q-8814b82fd0b2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2017-12-29&endDate=2017-12-31); parameters=`{"endDate": "2017-12-31", "startDate": "2017-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:53.184343Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:53.584962Z"}]`. Manifest query ID `8814b82fd0b2f67c21f783a9f6b27b6eec037788220f4ef6bd7efb984bb2acb9`.

<a id="q-f879c3f65b13541057d672ed63a5b9f8b1801322341a2761dbe21060a61ff3fe"></a>

FACT: Q-f879c3f65b13: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-01&endDate=2018-12-07); parameters=`{"endDate": "2018-12-07", "startDate": "2018-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:53.792209Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:54.085037Z"}]`. Manifest query ID `f879c3f65b13541057d672ed63a5b9f8b1801322341a2761dbe21060a61ff3fe`.

<a id="q-4bbef26ec32f48840f279b8326e3212c62d41766e3ef18fc936713b1a2103359"></a>

FACT: Q-4bbef26ec32f: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-08&endDate=2018-12-14); parameters=`{"endDate": "2018-12-14", "startDate": "2018-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:54.177626Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:54.585129Z"}]`. Manifest query ID `4bbef26ec32f48840f279b8326e3212c62d41766e3ef18fc936713b1a2103359`.

<a id="q-d8a6171589959c2cdd77e8cca8e42e05ae4c3d8809d5aa82ceb1a735e160ff88"></a>

FACT: Q-d8a617158995: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-15&endDate=2018-12-21); parameters=`{"endDate": "2018-12-21", "startDate": "2018-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:54.693145Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:55.085203Z"}]`. Manifest query ID `d8a6171589959c2cdd77e8cca8e42e05ae4c3d8809d5aa82ceb1a735e160ff88`.

<a id="q-70c75d3b5e7ed79b7c1b7230b70f8a9bac75ee759d7138adbf03efe251bb741c"></a>

FACT: Q-70c75d3b5e7e: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-22&endDate=2018-12-28); parameters=`{"endDate": "2018-12-28", "startDate": "2018-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:55.188790Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:55.585277Z"}]`. Manifest query ID `70c75d3b5e7ed79b7c1b7230b70f8a9bac75ee759d7138adbf03efe251bb741c`.

<a id="q-afb5d1a97b505004cbe28ab0cd10f607ee2fefa3d7db7bb8123336c6a9a7ab1b"></a>

FACT: Q-afb5d1a97b50: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2018-12-29&endDate=2018-12-31); parameters=`{"endDate": "2018-12-31", "startDate": "2018-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:55.662429Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:56.085351Z"}]`. Manifest query ID `afb5d1a97b505004cbe28ab0cd10f607ee2fefa3d7db7bb8123336c6a9a7ab1b`.

<a id="q-27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a"></a>

FACT: Q-27d37c5e2f42: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-01&endDate=2019-12-07); parameters=`{"endDate": "2019-12-07", "startDate": "2019-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:56.282974Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:56.585441Z"}]`. Manifest query ID `27d37c5e2f4243ebf502a2ebafdec234f25e0670f0a558225e9574aa7a42352a`.

<a id="q-e99ef1dee1093a44b99bd7015739384610d246125a4be8a5596548cd4b8ba8f2"></a>

FACT: Q-e99ef1dee109: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-08&endDate=2019-12-14); parameters=`{"endDate": "2019-12-14", "startDate": "2019-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:56.696090Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:57.085519Z"}]`. Manifest query ID `e99ef1dee1093a44b99bd7015739384610d246125a4be8a5596548cd4b8ba8f2`.

<a id="q-4b325fb28d97bd86f5e7f44cb9e5ee324f97a8f19129aaea2b8e371982da9719"></a>

FACT: Q-4b325fb28d97: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-15&endDate=2019-12-21); parameters=`{"endDate": "2019-12-21", "startDate": "2019-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:57.187468Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:57.585592Z"}]`. Manifest query ID `4b325fb28d97bd86f5e7f44cb9e5ee324f97a8f19129aaea2b8e371982da9719`.

<a id="q-c0d97ed051c2352f9461be18c5d9b7abed7b1d3a54a3dbbcbdf685bfe060a925"></a>

FACT: Q-c0d97ed051c2: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-22&endDate=2019-12-28); parameters=`{"endDate": "2019-12-28", "startDate": "2019-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:57.693979Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:58.085664Z"}]`. Manifest query ID `c0d97ed051c2352f9461be18c5d9b7abed7b1d3a54a3dbbcbdf685bfe060a925`.

<a id="q-4aebfb6dd7ec76d8e957293bd8acb9acb2dbf206a13075f3beb6941442953ae7"></a>

FACT: Q-4aebfb6dd7ec: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2019-12-29&endDate=2019-12-31); parameters=`{"endDate": "2019-12-31", "startDate": "2019-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:58.174083Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:58.585735Z"}]`. Manifest query ID `4aebfb6dd7ec76d8e957293bd8acb9acb2dbf206a13075f3beb6941442953ae7`.

<a id="q-2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70"></a>

FACT: Q-2c5d9ad70bdc: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2020-12-01&endDate=2020-12-07); parameters=`{"endDate": "2020-12-07", "startDate": "2020-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:58.834547Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:59.085811Z"}]`. Manifest query ID `2c5d9ad70bdc871cda3882ba4a7437aa0566bfb1286ee4cc0e87ccae5444ef70`.

<a id="q-e04a4e8d49ce851fa6c27ff10057930cd64f6d0aa2c291ed695f1a55f6e2bbda"></a>

FACT: Q-e04a4e8d49ce: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2020-12-08&endDate=2020-12-14); parameters=`{"endDate": "2020-12-14", "startDate": "2020-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:59.193117Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:35:59.585889Z"}]`. Manifest query ID `e04a4e8d49ce851fa6c27ff10057930cd64f6d0aa2c291ed695f1a55f6e2bbda`.

<a id="q-01ecfec456342435b221f8f68f7ffa850a3ab1fefe9113d30e97314b411c6977"></a>

FACT: Q-01ecfec45634: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2020-12-15&endDate=2020-12-21); parameters=`{"endDate": "2020-12-21", "startDate": "2020-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:35:59.711448Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:00.085984Z"}]`. Manifest query ID `01ecfec456342435b221f8f68f7ffa850a3ab1fefe9113d30e97314b411c6977`.

<a id="q-805d274a19a9f49fe09b06fe9188983bc8aa7d9486295a7fa0d3454e1de73515"></a>

FACT: Q-805d274a19a9: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2020-12-22&endDate=2020-12-28); parameters=`{"endDate": "2020-12-28", "startDate": "2020-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:00.186149Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:00.586031Z"}]`. Manifest query ID `805d274a19a9f49fe09b06fe9188983bc8aa7d9486295a7fa0d3454e1de73515`.

<a id="q-fd397dc3e730ee27a4a1d12d9ae6ea437095113c36949ef432876d50f1700567"></a>

FACT: Q-fd397dc3e730: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2020-12-29&endDate=2020-12-31); parameters=`{"endDate": "2020-12-31", "startDate": "2020-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:00.667988Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:01.086105Z"}]`. Manifest query ID `fd397dc3e730ee27a4a1d12d9ae6ea437095113c36949ef432876d50f1700567`.

<a id="q-e4d3ae0895edecb34cef8a6548bb80e7b7e8863cea9301c052fe2a67c4223bc4"></a>

FACT: Q-e4d3ae0895ed: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-01&endDate=2021-12-07); parameters=`{"endDate": "2021-12-07", "startDate": "2021-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:01.668063Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:01.668099Z"}]`. Manifest query ID `e4d3ae0895edecb34cef8a6548bb80e7b7e8863cea9301c052fe2a67c4223bc4`.

<a id="q-0e4cd9ba2efcf3ba0699665547a13e0d719fec4944e69b2b95609a228930366d"></a>

FACT: Q-0e4cd9ba2efc: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-08&endDate=2021-12-14); parameters=`{"endDate": "2021-12-14", "startDate": "2021-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:01.779992Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:02.168179Z"}]`. Manifest query ID `0e4cd9ba2efcf3ba0699665547a13e0d719fec4944e69b2b95609a228930366d`.

<a id="q-8a05cb012f015939fc8e42057023e43a057e665cebe3817f2870ee439f6c7d64"></a>

FACT: Q-8a05cb012f01: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-15&endDate=2021-12-21); parameters=`{"endDate": "2021-12-21", "startDate": "2021-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:02.280875Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:02.668252Z"}]`. Manifest query ID `8a05cb012f015939fc8e42057023e43a057e665cebe3817f2870ee439f6c7d64`.

<a id="q-ba0156e53a9a30df2ed1e2f1350193915f08de5e7de4b9225353ce50bb160464"></a>

FACT: Q-ba0156e53a9a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-22&endDate=2021-12-28); parameters=`{"endDate": "2021-12-28", "startDate": "2021-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:02.765466Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:03.168334Z"}]`. Manifest query ID `ba0156e53a9a30df2ed1e2f1350193915f08de5e7de4b9225353ce50bb160464`.

<a id="q-cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52"></a>

FACT: Q-cc532385b8b5: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2021-12-29&endDate=2021-12-31); parameters=`{"endDate": "2021-12-31", "startDate": "2021-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:03.258544Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:03.668410Z"}]`. Manifest query ID `cc532385b8b5243354460c9ccf8a40f3daf1e7ad572dd73080383fd150be1b52`.

<a id="q-2fbb208a9d374dc3045e6bb120b700c36d43cd10d5c71caa9f4fa1c80b9a0222"></a>

FACT: Q-2fbb208a9d37: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-01&endDate=2022-12-07); parameters=`{"endDate": "2022-12-07", "startDate": "2022-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:03.966778Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:04.168490Z"}]`. Manifest query ID `2fbb208a9d374dc3045e6bb120b700c36d43cd10d5c71caa9f4fa1c80b9a0222`.

<a id="q-01b69e34c5545e8a130038b64f2cdf4a185e8e44f0d9bef4c5b29f1f14ea6eb0"></a>

FACT: Q-01b69e34c554: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-08&endDate=2022-12-14); parameters=`{"endDate": "2022-12-14", "startDate": "2022-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:04.297905Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:04.668561Z"}]`. Manifest query ID `01b69e34c5545e8a130038b64f2cdf4a185e8e44f0d9bef4c5b29f1f14ea6eb0`.

<a id="q-18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b"></a>

FACT: Q-18f23a4cf0b6: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-15&endDate=2022-12-21); parameters=`{"endDate": "2022-12-21", "startDate": "2022-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:04.781659Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:05.168634Z"}]`. Manifest query ID `18f23a4cf0b6bfe9d24e97663b81355e37732d895a8caa6e72ef2e57c3c6db1b`.

<a id="q-2a9b6f78ee4062ba791fd3dce3a1bf5a22f39217c729712572afd2971321f86c"></a>

FACT: Q-2a9b6f78ee40: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-22&endDate=2022-12-28); parameters=`{"endDate": "2022-12-28", "startDate": "2022-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:05.300534Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:05.668710Z"}]`. Manifest query ID `2a9b6f78ee4062ba791fd3dce3a1bf5a22f39217c729712572afd2971321f86c`.

<a id="q-6f26709b0806358cb39c2a8964aec6bc14c22ec6b573d071387b483af9d84a81"></a>

FACT: Q-6f26709b0806: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2022-12-29&endDate=2022-12-31); parameters=`{"endDate": "2022-12-31", "startDate": "2022-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:05.777909Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:06.168785Z"}]`. Manifest query ID `6f26709b0806358cb39c2a8964aec6bc14c22ec6b573d071387b483af9d84a81`.

<a id="q-c2b708962c1aa44d3a7cce177a83cc6c2488ed63a32347ebcd5865a42ed813a3"></a>

FACT: Q-c2b708962c1a: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-01&endDate=2023-12-07); parameters=`{"endDate": "2023-12-07", "startDate": "2023-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:06.822770Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:06.822807Z"}]`. Manifest query ID `c2b708962c1aa44d3a7cce177a83cc6c2488ed63a32347ebcd5865a42ed813a3`.

<a id="q-24af1ee31dcfa11e4d414e44b7c2464693e9e3182528045ba4f62c6773a1af5b"></a>

FACT: Q-24af1ee31dcf: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-08&endDate=2023-12-14); parameters=`{"endDate": "2023-12-14", "startDate": "2023-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:06.952401Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:07.322894Z"}]`. Manifest query ID `24af1ee31dcfa11e4d414e44b7c2464693e9e3182528045ba4f62c6773a1af5b`.

<a id="q-dca79c9efd5c5232559662e41bed8fe0f77a7c152a464c17e17c71bd630ab38a"></a>

FACT: Q-dca79c9efd5c: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-15&endDate=2023-12-21); parameters=`{"endDate": "2023-12-21", "startDate": "2023-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:07.423753Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:07.822993Z"}]`. Manifest query ID `dca79c9efd5c5232559662e41bed8fe0f77a7c152a464c17e17c71bd630ab38a`.

<a id="q-777900a74a68d52773b2d9b912e306f466b2486dbb8e971701987edcce63a4e3"></a>

FACT: Q-777900a74a68: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-22&endDate=2023-12-28); parameters=`{"endDate": "2023-12-28", "startDate": "2023-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:07.940241Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:08.323034Z"}]`. Manifest query ID `777900a74a68d52773b2d9b912e306f466b2486dbb8e971701987edcce63a4e3`.

<a id="q-29721e95f74000bda38c5d50b713c3eaa1349f332368db89816c860cb1726cda"></a>

FACT: Q-29721e95f740: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2023-12-29&endDate=2023-12-31); parameters=`{"endDate": "2023-12-31", "startDate": "2023-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:08.410403Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:08.823112Z"}]`. Manifest query ID `29721e95f74000bda38c5d50b713c3eaa1349f332368db89816c860cb1726cda`.

<a id="q-833387c6a78802eabc66a6508a66fb766253d99c9778d343ee82e1c9c20dfc25"></a>

FACT: Q-833387c6a788: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-01&endDate=2024-12-07); parameters=`{"endDate": "2024-12-07", "startDate": "2024-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:09.199402Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:09.323165Z"}]`. Manifest query ID `833387c6a78802eabc66a6508a66fb766253d99c9778d343ee82e1c9c20dfc25`.

<a id="q-8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8"></a>

FACT: Q-8120005ec4aa: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-08&endDate=2024-12-14); parameters=`{"endDate": "2024-12-14", "startDate": "2024-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:09.430598Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:09.823246Z"}]`. Manifest query ID `8120005ec4aa98549b3dcf400e38f37590a3bbea265154e1dc1be7d8d492fbe8`.

<a id="q-20549587a2d82fd8f9264604f5d22265709b447ed75590d0c377e11165ff7b4a"></a>

FACT: Q-20549587a2d8: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-15&endDate=2024-12-21); parameters=`{"endDate": "2024-12-21", "startDate": "2024-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:09.991234Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:10.323323Z"}]`. Manifest query ID `20549587a2d82fd8f9264604f5d22265709b447ed75590d0c377e11165ff7b4a`.

<a id="q-5950de39204df7a9fc18604a10be1c579a11eed51296c89aaa0ee92391c77d83"></a>

FACT: Q-5950de39204d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-22&endDate=2024-12-28); parameters=`{"endDate": "2024-12-28", "startDate": "2024-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:10.462357Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:10.823403Z"}]`. Manifest query ID `5950de39204df7a9fc18604a10be1c579a11eed51296c89aaa0ee92391c77d83`.

<a id="q-6e2f84b370f723c027a4c92634cedb4e282f895d9b91ff9e9e0d4abfc0c37192"></a>

FACT: Q-6e2f84b370f7: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2024-12-29&endDate=2024-12-31); parameters=`{"endDate": "2024-12-31", "startDate": "2024-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:10.927603Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:11.323484Z"}]`. Manifest query ID `6e2f84b370f723c027a4c92634cedb4e282f895d9b91ff9e9e0d4abfc0c37192`.

<a id="q-4162b4a6197dcb0b9c1421f5105fbe7f5f622d34563f017697d843f598049b22"></a>

FACT: Q-4162b4a6197d: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-01&endDate=2025-12-07); parameters=`{"endDate": "2025-12-07", "startDate": "2025-12-01"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:11.728189Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:11.823558Z"}]`. Manifest query ID `4162b4a6197dcb0b9c1421f5105fbe7f5f622d34563f017697d843f598049b22`.

<a id="q-6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8"></a>

FACT: Q-6eb3a1b4eb6b: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-08&endDate=2025-12-14); parameters=`{"endDate": "2025-12-14", "startDate": "2025-12-08"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:11.921795Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:12.323636Z"}]`. Manifest query ID `6eb3a1b4eb6bbf1f2f56d4977260993142efecd8a85bf5adea8e2b080e3dc8e8`.

<a id="q-38f4a602f2df6574276469fc3d519c9684171bd70dcefc1725b1ba4083453723"></a>

FACT: Q-38f4a602f2df: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-15&endDate=2025-12-21); parameters=`{"endDate": "2025-12-21", "startDate": "2025-12-15"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:12.440518Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:12.823726Z"}]`. Manifest query ID `38f4a602f2df6574276469fc3d519c9684171bd70dcefc1725b1ba4083453723`.

<a id="q-36c1a33b7900376bd306750eff4cd6dbde603da187381f99185d63961906f7f9"></a>

FACT: Q-36c1a33b7900: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-22&endDate=2025-12-28); parameters=`{"endDate": "2025-12-28", "startDate": "2025-12-22"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:13.020025Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:13.323814Z"}]`. Manifest query ID `36c1a33b7900376bd306750eff4cd6dbde603da187381f99185d63961906f7f9`.

<a id="q-5e298169701732222fa28fc300ab125c3cecd505791fb565296c46dfb53c9572"></a>

FACT: Q-5e2981697017: [GET transactions](https://statsapi.mlb.com/api/v1/transactions?startDate=2025-12-29&endDate=2025-12-31); parameters=`{"endDate": "2025-12-31", "startDate": "2025-12-29"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:13.432465Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:13.823909Z"}]`. Manifest query ID `5e298169701732222fa28fc300ab125c3cecd505791fb565296c46dfb53c9572`.

<a id="q-d6685381152c5707ef63afb7d31dabfe00e2a438ceae3bf2797fce3ca6e96902"></a>

FACT: Q-d6685381152c: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2018&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:14.329376Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:14.329412Z"}]`. Manifest query ID `d6685381152c5707ef63afb7d31dabfe00e2a438ceae3bf2797fce3ca6e96902`.

<a id="q-67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e"></a>

FACT: Q-67237f081038: [GET people/658069/stats](https://statsapi.mlb.com/api/v1/people/658069/stats?stats=gameLog&group=hitting&season=2018&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:14.492832Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:14.829504Z"}]`. Manifest query ID `67237f081038e504ab5e6c49e7c8ca310c4935a7cf20140fcf954557e61ad80e`.

<a id="q-5d7a42d66ebac487c49d8c2a5b494685cc9b2a366f7fc50f959e9f732acfa778"></a>

FACT: Q-5d7a42d66eba: [GET people/658069/stats](https://statsapi.mlb.com/api/v1/people/658069/stats?stats=season&group=hitting&season=2018&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:14.901094Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:15.329586Z"}]`. Manifest query ID `5d7a42d66ebac487c49d8c2a5b494685cc9b2a366f7fc50f959e9f732acfa778`.

<a id="q-a5cde12acb991ebd49616937f1526423ca1cab2e34422582cbb7b7b6451ca40a"></a>

FACT: Q-a5cde12acb99: [GET people/658069/stats](https://statsapi.mlb.com/api/v1/people/658069/stats?stats=gameLog&group=hitting&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:15.411343Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:15.829660Z"}]`. Manifest query ID `a5cde12acb991ebd49616937f1526423ca1cab2e34422582cbb7b7b6451ca40a`.

<a id="q-0882d9c4e6456409491b6127cd4cbae28f045c9aa583fbcf5a613258ad41e213"></a>

FACT: Q-0882d9c4e645: [GET people/658069/stats](https://statsapi.mlb.com/api/v1/people/658069/stats?stats=gameLog&group=hitting&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:15.896968Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:16.329738Z"}]`. Manifest query ID `0882d9c4e6456409491b6127cd4cbae28f045c9aa583fbcf5a613258ad41e213`.

<a id="q-1de6ff58fbc755f6437515fb743b80bc7ecf9cb74b172e2e949bc4800c682fee"></a>

FACT: Q-1de6ff58fbc7: [GET people/658069/stats](https://statsapi.mlb.com/api/v1/people/658069/stats?stats=gameLog&group=hitting&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:16.392214Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:16.829822Z"}]`. Manifest query ID `1de6ff58fbc755f6437515fb743b80bc7ecf9cb74b172e2e949bc4800c682fee`.

<a id="q-7934e67dcaf516ef644ade8a547f9a6a2f4e4451648ca08a041b646effa75824"></a>

FACT: Q-7934e67dcaf5: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2018&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:16.893466Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:17.329897Z"}]`. Manifest query ID `7934e67dcaf516ef644ade8a547f9a6a2f4e4451648ca08a041b646effa75824`.

<a id="q-2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0"></a>

FACT: Q-2020931453c9: [GET people/474039/stats](https://statsapi.mlb.com/api/v1/people/474039/stats?stats=gameLog&group=pitching&season=2018&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:17.477625Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:17.829970Z"}]`. Manifest query ID `2020931453c98b4d388330c6f5a4c0d8e6fa7360ed58c6a999cff0f20b2fc3e0`.

<a id="q-3f8d3c0b7ed03ea57e76eb65f821844f3ffe877ed4f222424b3f69ef1b6aa4f7"></a>

FACT: Q-3f8d3c0b7ed0: [GET people/474039/stats](https://statsapi.mlb.com/api/v1/people/474039/stats?stats=season&group=pitching&season=2018&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:17.931592Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:18.330050Z"}]`. Manifest query ID `3f8d3c0b7ed03ea57e76eb65f821844f3ffe877ed4f222424b3f69ef1b6aa4f7`.

<a id="q-d2303de748fbf9f02a86f52e58d781f37a127865ec4420b447d2a70c72dd2d23"></a>

FACT: Q-d2303de748fb: [GET people/474039/stats](https://statsapi.mlb.com/api/v1/people/474039/stats?stats=gameLog&group=pitching&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:18.406875Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:18.830122Z"}]`. Manifest query ID `d2303de748fbf9f02a86f52e58d781f37a127865ec4420b447d2a70c72dd2d23`.

<a id="q-3cafdb0480e9a5a62d08c20198bc7aa54641c3354c3c293c98601bd67870ef7b"></a>

FACT: Q-3cafdb0480e9: [GET people/474039/stats](https://statsapi.mlb.com/api/v1/people/474039/stats?stats=gameLog&group=pitching&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:18.900361Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:19.330179Z"}]`. Manifest query ID `3cafdb0480e9a5a62d08c20198bc7aa54641c3354c3c293c98601bd67870ef7b`.

<a id="q-da5c3181a9669d894844cc8e11e1e5d75249936b0423de6193c63f43cd2091cc"></a>

FACT: Q-da5c3181a966: [GET people/474039/stats](https://statsapi.mlb.com/api/v1/people/474039/stats?stats=gameLog&group=pitching&season=2018&sportIds=11&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 11, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:19.395471Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:19.830251Z"}]`. Manifest query ID `da5c3181a9669d894844cc8e11e1e5d75249936b0423de6193c63f43cd2091cc`.

<a id="q-581398926defa230a4f86546485d7916fcebaeb25ef362099666b6f3615111fd"></a>

FACT: Q-581398926def: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2018&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:19.892933Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:20.330331Z"}]`. Manifest query ID `581398926defa230a4f86546485d7916fcebaeb25ef362099666b6f3615111fd`.

<a id="q-d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0"></a>

FACT: Q-d57489e11662: [GET people/656509/stats](https://statsapi.mlb.com/api/v1/people/656509/stats?stats=gameLog&group=hitting&season=2018&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:20.488111Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:20.830401Z"}]`. Manifest query ID `d57489e11662d3af833778d3bd768c30517b0a0fd3f9581934f3b518f8d329a0`.

<a id="q-5c94e9b959e42a967b5d10f1a9cefcf2bc916b36c1689f72570e87b700042e65"></a>

FACT: Q-5c94e9b959e4: [GET people/656509/stats](https://statsapi.mlb.com/api/v1/people/656509/stats?stats=season&group=hitting&season=2018&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:21.429203Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:21.429231Z"}]`. Manifest query ID `5c94e9b959e42a967b5d10f1a9cefcf2bc916b36c1689f72570e87b700042e65`.

<a id="q-d0d465f2e98873f2037e4be4449df58f743d964bd9780334c9b995c8dc472f10"></a>

FACT: Q-d0d465f2e988: [GET people/656509/stats](https://statsapi.mlb.com/api/v1/people/656509/stats?stats=gameLog&group=hitting&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:21.499813Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:21.929314Z"}]`. Manifest query ID `d0d465f2e98873f2037e4be4449df58f743d964bd9780334c9b995c8dc472f10`.

<a id="q-c35ec0b3f822de91395a6063fb35d4c91dba8f23485f6bc810330fbeea761cf8"></a>

FACT: Q-c35ec0b3f822: [GET people/656509/stats](https://statsapi.mlb.com/api/v1/people/656509/stats?stats=gameLog&group=hitting&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:21.992101Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:22.429391Z"}]`. Manifest query ID `c35ec0b3f822de91395a6063fb35d4c91dba8f23485f6bc810330fbeea761cf8`.

<a id="q-adfb9653b818702a1eb373f2779fd7576f584d1c1e00243cbd93a0acb675ba09"></a>

FACT: Q-adfb9653b818: [GET people/656509/stats](https://statsapi.mlb.com/api/v1/people/656509/stats?stats=gameLog&group=hitting&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:22.500587Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:22.929482Z"}]`. Manifest query ID `adfb9653b818702a1eb373f2779fd7576f584d1c1e00243cbd93a0acb675ba09`.

<a id="q-0c60fd1ce45ea001d1179d3523359456f4b979bb8f8bf5bcb0735f41259e442c"></a>

FACT: Q-0c60fd1ce45e: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2018&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:22.994110Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:23.429560Z"}]`. Manifest query ID `0c60fd1ce45ea001d1179d3523359456f4b979bb8f8bf5bcb0735f41259e442c`.

<a id="q-50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f"></a>

FACT: Q-50806df9dc86: [GET people/641980/stats](https://statsapi.mlb.com/api/v1/people/641980/stats?stats=gameLog&group=pitching&season=2018&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:23.585060Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:23.929635Z"}]`. Manifest query ID `50806df9dc869417695e291bdef905eff0c5fd5b56418ee408e4c0b857f7888f`.

<a id="q-945ed6765c9f72f87b1c2cb55419ddfb0d517c956d95d6fac571ad7f2360ea34"></a>

FACT: Q-945ed6765c9f: [GET people/641980/stats](https://statsapi.mlb.com/api/v1/people/641980/stats?stats=season&group=pitching&season=2018&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:24.004335Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:24.429708Z"}]`. Manifest query ID `945ed6765c9f72f87b1c2cb55419ddfb0d517c956d95d6fac571ad7f2360ea34`.

<a id="q-7ea60465bd00afeb99ff6c0c52208f9f7a870cbeaa1929dae8fbe6cee8319591"></a>

FACT: Q-7ea60465bd00: [GET people/641980/stats](https://statsapi.mlb.com/api/v1/people/641980/stats?stats=gameLog&group=pitching&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:24.507959Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:24.929782Z"}]`. Manifest query ID `7ea60465bd00afeb99ff6c0c52208f9f7a870cbeaa1929dae8fbe6cee8319591`.

<a id="q-19df4c90fefe44c756ddec329111f546cf608cc58f8dafcdbc9ce3310b5baa7c"></a>

FACT: Q-19df4c90fefe: [GET people/641980/stats](https://statsapi.mlb.com/api/v1/people/641980/stats?stats=gameLog&group=pitching&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:24.995895Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:25.429866Z"}]`. Manifest query ID `19df4c90fefe44c756ddec329111f546cf608cc58f8dafcdbc9ce3310b5baa7c`.

<a id="q-a3bcd1c95efe59b68d920f96aa0d6e23b97fa7cfafacf04eb2b4bd5e4e4860e9"></a>

FACT: Q-a3bcd1c95efe: [GET people/641980/stats](https://statsapi.mlb.com/api/v1/people/641980/stats?stats=gameLog&group=pitching&season=2018&sportIds=12&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 12, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:25.504229Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:25.929937Z"}]`. Manifest query ID `a3bcd1c95efe59b68d920f96aa0d6e23b97fa7cfafacf04eb2b4bd5e4e4860e9`.

<a id="q-560c8f9b8fffd4b2d5f7ed57c036939ec868e5c6a8b51e217c7b38952e2ca00d"></a>

FACT: Q-560c8f9b8fff: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2018&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:25.995995Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:26.430033Z"}]`. Manifest query ID `560c8f9b8fffd4b2d5f7ed57c036939ec868e5c6a8b51e217c7b38952e2ca00d`.

<a id="q-fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367"></a>

FACT: Q-fb194b4f299d: [GET people/676631/stats](https://statsapi.mlb.com/api/v1/people/676631/stats?stats=gameLog&group=hitting&season=2018&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:26.552948Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:26.930109Z"}]`. Manifest query ID `fb194b4f299d1b00865174b146470e405ea8b2b3cdae16bd358c8c63db192367`.

<a id="q-c8a24f4c0edbc317e7e0b1614ddbc7cd916869cff76588c9b346dc218666c214"></a>

FACT: Q-c8a24f4c0edb: [GET people/676631/stats](https://statsapi.mlb.com/api/v1/people/676631/stats?stats=season&group=hitting&season=2018&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:27.485612Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:27.485635Z"}]`. Manifest query ID `c8a24f4c0edbc317e7e0b1614ddbc7cd916869cff76588c9b346dc218666c214`.

<a id="q-24216ebc77a1f829db9c064052058c60a56cd0448a1e9b96fa9942b3a2cb26e7"></a>

FACT: Q-24216ebc77a1: [GET people/676631/stats](https://statsapi.mlb.com/api/v1/people/676631/stats?stats=gameLog&group=hitting&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:27.555050Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:27.985728Z"}]`. Manifest query ID `24216ebc77a1f829db9c064052058c60a56cd0448a1e9b96fa9942b3a2cb26e7`.

<a id="q-4c812c133059a0f756f02997789e5d0c833d7ebf8729771bb3d3d63b2be499aa"></a>

FACT: Q-4c812c133059: [GET people/676631/stats](https://statsapi.mlb.com/api/v1/people/676631/stats?stats=gameLog&group=hitting&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:28.049831Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:28.485816Z"}]`. Manifest query ID `4c812c133059a0f756f02997789e5d0c833d7ebf8729771bb3d3d63b2be499aa`.

<a id="q-f9dde909ddbc3a2812d087cfdd00ca4d65bdc10ba6d873db494cf6567000eed7"></a>

FACT: Q-f9dde909ddbc: [GET people/676631/stats](https://statsapi.mlb.com/api/v1/people/676631/stats?stats=gameLog&group=hitting&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:28.553853Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:28.985907Z"}]`. Manifest query ID `f9dde909ddbc3a2812d087cfdd00ca4d65bdc10ba6d873db494cf6567000eed7`.

<a id="q-9ce0475643c6d326a1378423bd1827703fe6bf9f2b69beab40f2dc1f6f852ab7"></a>

FACT: Q-9ce0475643c6: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2018&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:29.062648Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:29.485981Z"}]`. Manifest query ID `9ce0475643c6d326a1378423bd1827703fe6bf9f2b69beab40f2dc1f6f852ab7`.

<a id="q-d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f"></a>

FACT: Q-d579d931485e: [GET people/664875/stats](https://statsapi.mlb.com/api/v1/people/664875/stats?stats=gameLog&group=pitching&season=2018&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:29.650653Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:29.986045Z"}]`. Manifest query ID `d579d931485e4b0af8e3462458e33fbc3756d39faf3320870c091963c1e72d3f`.

<a id="q-064c1d9360ac42130052eb754f99aac18574d08de906b000d4e5fc2d6b419861"></a>

FACT: Q-064c1d9360ac: [GET people/664875/stats](https://statsapi.mlb.com/api/v1/people/664875/stats?stats=season&group=pitching&season=2018&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:30.051053Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:30.486120Z"}]`. Manifest query ID `064c1d9360ac42130052eb754f99aac18574d08de906b000d4e5fc2d6b419861`.

<a id="q-caed2fbc55a13e230367908da416cb4a69a98acc5109dd110656d6cb20a875b3"></a>

FACT: Q-caed2fbc55a1: [GET people/664875/stats](https://statsapi.mlb.com/api/v1/people/664875/stats?stats=gameLog&group=pitching&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:30.560588Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:30.986198Z"}]`. Manifest query ID `caed2fbc55a13e230367908da416cb4a69a98acc5109dd110656d6cb20a875b3`.

<a id="q-e91d1fffdd972215ba8d808746241131097000a8a4c3fc9f5a3722f2708f8e51"></a>

FACT: Q-e91d1fffdd97: [GET people/664875/stats](https://statsapi.mlb.com/api/v1/people/664875/stats?stats=gameLog&group=pitching&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:31.055028Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:31.486280Z"}]`. Manifest query ID `e91d1fffdd972215ba8d808746241131097000a8a4c3fc9f5a3722f2708f8e51`.

<a id="q-847e199d8aaef93f246d4ae9bd8c9ca4ed7499c1a2fae09b962c58540f138de5"></a>

FACT: Q-847e199d8aae: [GET people/664875/stats](https://statsapi.mlb.com/api/v1/people/664875/stats?stats=gameLog&group=pitching&season=2018&sportIds=13&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 13, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:31.554307Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:31.986354Z"}]`. Manifest query ID `847e199d8aaef93f246d4ae9bd8c9ca4ed7499c1a2fae09b962c58540f138de5`.

<a id="q-47b6403864a453553f3a34f4ed8edcb2d1e4107a4e914586b5c5da5e56aceabb"></a>

FACT: Q-47b6403864a4: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2018&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:32.051898Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:32.486432Z"}]`. Manifest query ID `47b6403864a453553f3a34f4ed8edcb2d1e4107a4e914586b5c5da5e56aceabb`.

<a id="q-025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7"></a>

FACT: Q-025b6373402d: [GET people/650626/stats](https://statsapi.mlb.com/api/v1/people/650626/stats?stats=gameLog&group=hitting&season=2018&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:32.622353Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:32.986515Z"}]`. Manifest query ID `025b6373402debb31e81e4fac69de84421aa526531886bc249f097b9d3dfdca7`.

<a id="q-a65420fedb48a76b3ef23f4da536ddaa636ae53f0ed8aa9084862293ea7007ce"></a>

FACT: Q-a65420fedb48: [GET people/650626/stats](https://statsapi.mlb.com/api/v1/people/650626/stats?stats=season&group=hitting&season=2018&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2018, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:33.073289Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:33.486586Z"}]`. Manifest query ID `a65420fedb48a76b3ef23f4da536ddaa636ae53f0ed8aa9084862293ea7007ce`.

<a id="q-9bf6e7d0a6e58f282b71c62ac5ef3a8bc5f1c5302372b12168acb3fce24ebd8b"></a>

FACT: Q-9bf6e7d0a6e5: [GET people/650626/stats](https://statsapi.mlb.com/api/v1/people/650626/stats?stats=gameLog&group=hitting&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:33.553918Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:33.986635Z"}]`. Manifest query ID `9bf6e7d0a6e58f282b71c62ac5ef3a8bc5f1c5302372b12168acb3fce24ebd8b`.

<a id="q-08ea9cdea83f299d3202cf50a46474de263dcc91dd5b2b4c74176361d752fceb"></a>

FACT: Q-08ea9cdea83f: [GET people/650626/stats](https://statsapi.mlb.com/api/v1/people/650626/stats?stats=gameLog&group=hitting&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:34.051729Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:34.486726Z"}]`. Manifest query ID `08ea9cdea83f299d3202cf50a46474de263dcc91dd5b2b4c74176361d752fceb`.

<a id="q-990b17b8d0dc352cda25a7ad5c9ff863b6d7425a2ddcae2d80d4b59fbb17be11"></a>

FACT: Q-990b17b8d0dc: [GET people/650626/stats](https://statsapi.mlb.com/api/v1/people/650626/stats?stats=gameLog&group=hitting&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "hitting", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:34.557597Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:34.986797Z"}]`. Manifest query ID `990b17b8d0dc352cda25a7ad5c9ff863b6d7425a2ddcae2d80d4b59fbb17be11`.

<a id="q-e1a7f586e0bd72bc3537c17bf07786e0c89cca9613cec69f2596cc192d0dcf73"></a>

FACT: Q-e1a7f586e0bd: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2018&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2018, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:35.060803Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:35.486879Z"}]`. Manifest query ID `e1a7f586e0bd72bc3537c17bf07786e0c89cca9613cec69f2596cc192d0dcf73`.

<a id="q-d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196"></a>

FACT: Q-d12ae0a22d3f: [GET people/656382/stats](https://statsapi.mlb.com/api/v1/people/656382/stats?stats=gameLog&group=pitching&season=2018&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:35.732364Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:35.986952Z"}]`. Manifest query ID `d12ae0a22d3f1b2d2a8029ff4c8cd5e6080f5f3bdf4e828a593f3f9615f6f196`.

<a id="q-eaf82a7b051662ac916b7f3ac23338e13146719321b4ff064985158eaeff29e2"></a>

FACT: Q-eaf82a7b0516: [GET people/656382/stats](https://statsapi.mlb.com/api/v1/people/656382/stats?stats=season&group=pitching&season=2018&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2018, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:36.059272Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:36.487034Z"}]`. Manifest query ID `eaf82a7b051662ac916b7f3ac23338e13146719321b4ff064985158eaeff29e2`.

<a id="q-e27f26d86cb8c887cfe36c6eb7bae76651d1f4c3eeda4836ae60c5b89380b964"></a>

FACT: Q-e27f26d86cb8: [GET people/656382/stats](https://statsapi.mlb.com/api/v1/people/656382/stats?stats=gameLog&group=pitching&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-05-31); parameters=`{"endDate": "2018-05-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:36.570397Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:36.987111Z"}]`. Manifest query ID `e27f26d86cb8c887cfe36c6eb7bae76651d1f4c3eeda4836ae60c5b89380b964`.

<a id="q-39c9d35f3b367c088ebe105effe12332a8dc2060c9e80de4a997d453158078b3"></a>

FACT: Q-39c9d35f3b36: [GET people/656382/stats](https://statsapi.mlb.com/api/v1/people/656382/stats?stats=gameLog&group=pitching&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-07-15); parameters=`{"endDate": "2018-07-15", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:37.058989Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:37.487184Z"}]`. Manifest query ID `39c9d35f3b367c088ebe105effe12332a8dc2060c9e80de4a997d453158078b3`.

<a id="q-01e01ced12b24015763725e7cdfc434a0cc549927714f8706649f4ac7fefda5e"></a>

FACT: Q-01e01ced12b2: [GET people/656382/stats](https://statsapi.mlb.com/api/v1/people/656382/stats?stats=gameLog&group=pitching&season=2018&sportIds=14&gameType=R&startDate=2018-01-01&endDate=2018-08-31); parameters=`{"endDate": "2018-08-31", "gameType": "R", "group": "pitching", "season": 2018, "sportIds": 14, "startDate": "2018-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:37.556466Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:37.987261Z"}]`. Manifest query ID `01e01ced12b24015763725e7cdfc434a0cc549927714f8706649f4ac7fefda5e`.

<a id="q-7a3be1224d1b7c0f4a6da18cf49145e4f4f058cd2801033898bee20f0fb2caf4"></a>

FACT: Q-7a3be1224d1b: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2019&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:38.068819Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:38.487334Z"}]`. Manifest query ID `7a3be1224d1b7c0f4a6da18cf49145e4f4f058cd2801033898bee20f0fb2caf4`.

<a id="q-12c7eff3f88e249fadffb4911ab7a08044819970a76b3c823c9ae03f61a24b26"></a>

FACT: Q-12c7eff3f88e: [GET people/518568/stats](https://statsapi.mlb.com/api/v1/people/518568/stats?stats=gameLog&group=hitting&season=2019&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:38.662933Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:38.987409Z"}]`. Manifest query ID `12c7eff3f88e249fadffb4911ab7a08044819970a76b3c823c9ae03f61a24b26`.

<a id="q-ab0a1cd85e5f39212fa09d74004e6ef71e7082220892231860bf2a006b442c96"></a>

FACT: Q-ab0a1cd85e5f: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2019&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:39.066616Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:39.487484Z"}]`. Manifest query ID `ab0a1cd85e5f39212fa09d74004e6ef71e7082220892231860bf2a006b442c96`.

<a id="q-cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada"></a>

FACT: Q-cbcb327b8e18: [GET people/662964/stats](https://statsapi.mlb.com/api/v1/people/662964/stats?stats=gameLog&group=pitching&season=2019&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:39.781332Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:39.987592Z"}]`. Manifest query ID `cbcb327b8e18399b018b1ed38997425e87974098e85e49e660b7963c05d4bada`.

<a id="q-a6cb4395ed559593e9f0b23cffce7d7887764622f1335e9a0473d4642b72dcea"></a>

FACT: Q-a6cb4395ed55: [GET people/662964/stats](https://statsapi.mlb.com/api/v1/people/662964/stats?stats=season&group=pitching&season=2019&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:40.084649Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:40.487681Z"}]`. Manifest query ID `a6cb4395ed559593e9f0b23cffce7d7887764622f1335e9a0473d4642b72dcea`.

<a id="q-dc6e9528dde89878ce3a07dd42cd41d1bc66bcca24eb28b321bb807d3aac3be3"></a>

FACT: Q-dc6e9528dde8: [GET people/662964/stats](https://statsapi.mlb.com/api/v1/people/662964/stats?stats=gameLog&group=pitching&season=2019&sportIds=11&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 11, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:40.572141Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:40.987774Z"}]`. Manifest query ID `dc6e9528dde89878ce3a07dd42cd41d1bc66bcca24eb28b321bb807d3aac3be3`.

<a id="q-710219887d4422be4c031d418c9dfa6ddce431ac5bd65ee0330bb4c5ef0c06bd"></a>

FACT: Q-710219887d44: [GET people/662964/stats](https://statsapi.mlb.com/api/v1/people/662964/stats?stats=gameLog&group=pitching&season=2019&sportIds=11&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 11, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:41.070418Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:41.487863Z"}]`. Manifest query ID `710219887d4422be4c031d418c9dfa6ddce431ac5bd65ee0330bb4c5ef0c06bd`.

<a id="q-54a579b20cd1d399eb3e2ced80d8adf86bfec38df15446fea38f6064ad805bcb"></a>

FACT: Q-54a579b20cd1: [GET people/662964/stats](https://statsapi.mlb.com/api/v1/people/662964/stats?stats=gameLog&group=pitching&season=2019&sportIds=11&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 11, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:41.562116Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:41.987977Z"}]`. Manifest query ID `54a579b20cd1d399eb3e2ced80d8adf86bfec38df15446fea38f6064ad805bcb`.

<a id="q-8dbc9bae91d6ec2f11be357abfc423cd098b5dd6981a256469fc4c73388e625c"></a>

FACT: Q-8dbc9bae91d6: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2019&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:42.066059Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:42.488080Z"}]`. Manifest query ID `8dbc9bae91d6ec2f11be357abfc423cd098b5dd6981a256469fc4c73388e625c`.

<a id="q-961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1"></a>

FACT: Q-961af1d0d72a: [GET people/663630/stats](https://statsapi.mlb.com/api/v1/people/663630/stats?stats=gameLog&group=hitting&season=2019&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:42.778686Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:42.988197Z"}]`. Manifest query ID `961af1d0d72a54cfeb2b0ba4c56d1c961d61103ed932f4cf61924b2c61aab5e1`.

<a id="q-0651a1e98903510b69f02fa732280b51d0679ac3f1073daad353be92a7661ae3"></a>

FACT: Q-0651a1e98903: [GET people/663630/stats](https://statsapi.mlb.com/api/v1/people/663630/stats?stats=season&group=hitting&season=2019&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:43.562704Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:43.562732Z"}]`. Manifest query ID `0651a1e98903510b69f02fa732280b51d0679ac3f1073daad353be92a7661ae3`.

<a id="q-6c5419795a8a71d88468c45c68fc0e014f6d947f90d5160f8bd70cf381205233"></a>

FACT: Q-6c5419795a8a: [GET people/663630/stats](https://statsapi.mlb.com/api/v1/people/663630/stats?stats=gameLog&group=hitting&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:43.623205Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:44.062813Z"}]`. Manifest query ID `6c5419795a8a71d88468c45c68fc0e014f6d947f90d5160f8bd70cf381205233`.

<a id="q-35de6b176d4ae37717ade5d86fbb9747a3a00a5ee63b33628593c3624a73c38b"></a>

FACT: Q-35de6b176d4a: [GET people/663630/stats](https://statsapi.mlb.com/api/v1/people/663630/stats?stats=gameLog&group=hitting&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:44.132317Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:44.562889Z"}]`. Manifest query ID `35de6b176d4ae37717ade5d86fbb9747a3a00a5ee63b33628593c3624a73c38b`.

<a id="q-dcc7c90595a18bfb85e8b82b24e7ce36661741623ecdb3bbdf331d490bcb60e4"></a>

FACT: Q-dcc7c90595a1: [GET people/663630/stats](https://statsapi.mlb.com/api/v1/people/663630/stats?stats=gameLog&group=hitting&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:44.630816Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:45.062973Z"}]`. Manifest query ID `dcc7c90595a18bfb85e8b82b24e7ce36661741623ecdb3bbdf331d490bcb60e4`.

<a id="q-393421dc4d2a8c82c3eaa83dcb4d7354ecdaafa093652bb3592ca22758e009b0"></a>

FACT: Q-393421dc4d2a: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2019&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:45.136476Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:45.563047Z"}]`. Manifest query ID `393421dc4d2a8c82c3eaa83dcb4d7354ecdaafa093652bb3592ca22758e009b0`.

<a id="q-0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205"></a>

FACT: Q-0b4341ae0cfc: [GET people/676754/stats](https://statsapi.mlb.com/api/v1/people/676754/stats?stats=gameLog&group=pitching&season=2019&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:45.730854Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:46.063121Z"}]`. Manifest query ID `0b4341ae0cfc6859826051f5b67e0f54c03d5969f2e1a32fc49e6a08cfa8a205`.

<a id="q-f1b0b1131afa443e51f04c53945c42f9a84b8498e1ac4621f4bb8ad413f30ebb"></a>

FACT: Q-f1b0b1131afa: [GET people/676754/stats](https://statsapi.mlb.com/api/v1/people/676754/stats?stats=season&group=pitching&season=2019&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:46.135873Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:46.563197Z"}]`. Manifest query ID `f1b0b1131afa443e51f04c53945c42f9a84b8498e1ac4621f4bb8ad413f30ebb`.

<a id="q-5c373e62a4a2e544b9caf7f969e6d5d335802d137bf83e4a648df1499ce3bc89"></a>

FACT: Q-5c373e62a4a2: [GET people/676754/stats](https://statsapi.mlb.com/api/v1/people/676754/stats?stats=gameLog&group=pitching&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:46.639190Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:47.063270Z"}]`. Manifest query ID `5c373e62a4a2e544b9caf7f969e6d5d335802d137bf83e4a648df1499ce3bc89`.

<a id="q-10efde1bcc0671f533172b2fe9836dee0a413eaa66e209148306e4bfa25593c5"></a>

FACT: Q-10efde1bcc06: [GET people/676754/stats](https://statsapi.mlb.com/api/v1/people/676754/stats?stats=gameLog&group=pitching&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:47.128372Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:47.563347Z"}]`. Manifest query ID `10efde1bcc0671f533172b2fe9836dee0a413eaa66e209148306e4bfa25593c5`.

<a id="q-4c69d5d20b26277b6f01345256be8ef3ce1b28d7ae86e638a5257cd71c4b7082"></a>

FACT: Q-4c69d5d20b26: [GET people/676754/stats](https://statsapi.mlb.com/api/v1/people/676754/stats?stats=gameLog&group=pitching&season=2019&sportIds=12&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 12, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:47.640447Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:48.063420Z"}]`. Manifest query ID `4c69d5d20b26277b6f01345256be8ef3ce1b28d7ae86e638a5257cd71c4b7082`.

<a id="q-74d5d3a2ba25df5e9ea10b165aac7fcb6d543121e45c9bb4add1f6a85ae883b7"></a>

FACT: Q-74d5d3a2ba25: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2019&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:48.131688Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:48.563493Z"}]`. Manifest query ID `74d5d3a2ba25df5e9ea10b165aac7fcb6d543121e45c9bb4add1f6a85ae883b7`.

<a id="q-95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e"></a>

FACT: Q-95b4ddeae5c8: [GET people/677690/stats](https://statsapi.mlb.com/api/v1/people/677690/stats?stats=gameLog&group=hitting&season=2019&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:48.698096Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:49.063573Z"}]`. Manifest query ID `95b4ddeae5c887f14036073468202f4dbd59f4c1c0da6b0f73c558770e053e4e`.

<a id="q-9aafdeae8e856f976cbbc8875fb0b716ca4ec697682b8c608621af6366ff17dd"></a>

FACT: Q-9aafdeae8e85: [GET people/677690/stats](https://statsapi.mlb.com/api/v1/people/677690/stats?stats=season&group=hitting&season=2019&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:49.165260Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:49.563646Z"}]`. Manifest query ID `9aafdeae8e856f976cbbc8875fb0b716ca4ec697682b8c608621af6366ff17dd`.

<a id="q-84b8d49b0dee99e18ca17d21b6301e17cdccc1fa82bd976f9c701f55541e9fc0"></a>

FACT: Q-84b8d49b0dee: [GET people/677690/stats](https://statsapi.mlb.com/api/v1/people/677690/stats?stats=gameLog&group=hitting&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:49.627744Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:50.063752Z"}]`. Manifest query ID `84b8d49b0dee99e18ca17d21b6301e17cdccc1fa82bd976f9c701f55541e9fc0`.

<a id="q-f2db68df06a8d81dd777f3417ceb5cf51f6e4d78d260858818f78a96ba156bb9"></a>

FACT: Q-f2db68df06a8: [GET people/677690/stats](https://statsapi.mlb.com/api/v1/people/677690/stats?stats=gameLog&group=hitting&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:50.130536Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:50.563828Z"}]`. Manifest query ID `f2db68df06a8d81dd777f3417ceb5cf51f6e4d78d260858818f78a96ba156bb9`.

<a id="q-b774d3657cb87a700d2d618d24e7dc1c398e75f7b25cc914d18b8eb6a08386b6"></a>

FACT: Q-b774d3657cb8: [GET people/677690/stats](https://statsapi.mlb.com/api/v1/people/677690/stats?stats=gameLog&group=hitting&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:50.628107Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:51.063901Z"}]`. Manifest query ID `b774d3657cb87a700d2d618d24e7dc1c398e75f7b25cc914d18b8eb6a08386b6`.

<a id="q-6c43784a9ee799450777efef71b9a6ef67fe067161d43c6b45edd0b7cdb524d3"></a>

FACT: Q-6c43784a9ee7: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2019&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:51.131861Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:51.563991Z"}]`. Manifest query ID `6c43784a9ee799450777efef71b9a6ef67fe067161d43c6b45edd0b7cdb524d3`.

<a id="q-e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1"></a>

FACT: Q-e3df5fba44ad: [GET people/670442/stats](https://statsapi.mlb.com/api/v1/people/670442/stats?stats=gameLog&group=pitching&season=2019&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:51.874310Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:52.064039Z"}]`. Manifest query ID `e3df5fba44ad0762599002ccb8f13844d6d9e89a798444cbcbb4024b6d261ab1`.

<a id="q-c95a73ecccb592e651fd4c9e77aca5d44811e649d25d82738cdbd99ba7f465e0"></a>

FACT: Q-c95a73ecccb5: [GET people/670442/stats](https://statsapi.mlb.com/api/v1/people/670442/stats?stats=season&group=pitching&season=2019&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:52.141045Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:52.564119Z"}]`. Manifest query ID `c95a73ecccb592e651fd4c9e77aca5d44811e649d25d82738cdbd99ba7f465e0`.

<a id="q-e117766f09a7b650a26b0f2fe5dc406de4c55aeacb8fa2ad9229fba612a447e1"></a>

FACT: Q-e117766f09a7: [GET people/670442/stats](https://statsapi.mlb.com/api/v1/people/670442/stats?stats=gameLog&group=pitching&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:52.641834Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:53.064206Z"}]`. Manifest query ID `e117766f09a7b650a26b0f2fe5dc406de4c55aeacb8fa2ad9229fba612a447e1`.

<a id="q-6c5b5e8ad9f73fbd8eacaa38b750d0f13276c00da831f0cc478637c94b0d646e"></a>

FACT: Q-6c5b5e8ad9f7: [GET people/670442/stats](https://statsapi.mlb.com/api/v1/people/670442/stats?stats=gameLog&group=pitching&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:53.129104Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:53.564278Z"}]`. Manifest query ID `6c5b5e8ad9f73fbd8eacaa38b750d0f13276c00da831f0cc478637c94b0d646e`.

<a id="q-bd5a95850e345dab11ee395943abe479876d574db316cdc756d2b8c6fdbc670f"></a>

FACT: Q-bd5a95850e34: [GET people/670442/stats](https://statsapi.mlb.com/api/v1/people/670442/stats?stats=gameLog&group=pitching&season=2019&sportIds=13&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 13, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:53.634069Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:54.064353Z"}]`. Manifest query ID `bd5a95850e345dab11ee395943abe479876d574db316cdc756d2b8c6fdbc670f`.

<a id="q-8de868442d9592f43793d874b35a0a5c74021316275fdd3b7e95313300830953"></a>

FACT: Q-8de868442d95: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2019&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:54.134598Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:54.564428Z"}]`. Manifest query ID `8de868442d9592f43793d874b35a0a5c74021316275fdd3b7e95313300830953`.

<a id="q-a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f"></a>

FACT: Q-a7d1f3ca02a1: [GET people/681807/stats](https://statsapi.mlb.com/api/v1/people/681807/stats?stats=gameLog&group=hitting&season=2019&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:54.689741Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:55.064501Z"}]`. Manifest query ID `a7d1f3ca02a1a92306206a1e48a2e07b0ea622a66441d7390d62361a9a65472f`.

<a id="q-1ea770d6e87639c39c30f99d9d8b4efb0cd7ac3e3db5bb1a4947cc05f66ef211"></a>

FACT: Q-1ea770d6e876: [GET people/681807/stats](https://statsapi.mlb.com/api/v1/people/681807/stats?stats=season&group=hitting&season=2019&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2019, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:55.139853Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:55.564582Z"}]`. Manifest query ID `1ea770d6e87639c39c30f99d9d8b4efb0cd7ac3e3db5bb1a4947cc05f66ef211`.

<a id="q-5f953bfec3f554f3493e988e18fe754cb762e19ffa4ded9f962f040aee206b6c"></a>

FACT: Q-5f953bfec3f5: [GET people/681807/stats](https://statsapi.mlb.com/api/v1/people/681807/stats?stats=gameLog&group=hitting&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:55.631761Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:56.064669Z"}]`. Manifest query ID `5f953bfec3f554f3493e988e18fe754cb762e19ffa4ded9f962f040aee206b6c`.

<a id="q-822f42d2e3dfe16a8e429d633f565af12108cdd2f21df10b18d2f7d7f49503bc"></a>

FACT: Q-822f42d2e3df: [GET people/681807/stats](https://statsapi.mlb.com/api/v1/people/681807/stats?stats=gameLog&group=hitting&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:56.145646Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:56.564744Z"}]`. Manifest query ID `822f42d2e3dfe16a8e429d633f565af12108cdd2f21df10b18d2f7d7f49503bc`.

<a id="q-ecee20b0ecf4387e673664daa1aff4faefbe1eadc94d7b714d20168a387947ae"></a>

FACT: Q-ecee20b0ecf4: [GET people/681807/stats](https://statsapi.mlb.com/api/v1/people/681807/stats?stats=gameLog&group=hitting&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "hitting", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:56.630136Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:57.064820Z"}]`. Manifest query ID `ecee20b0ecf4387e673664daa1aff4faefbe1eadc94d7b714d20168a387947ae`.

<a id="q-7d5b8f5b799d8526ee673098b94dea3c1d000a9ca9ce468f711c7da8ab84933f"></a>

FACT: Q-7d5b8f5b799d: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2019&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2019, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:57.126353Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:57.564909Z"}]`. Manifest query ID `7d5b8f5b799d8526ee673098b94dea3c1d000a9ca9ce468f711c7da8ab84933f`.

<a id="q-c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223"></a>

FACT: Q-c42229e15b50: [GET people/676571/stats](https://statsapi.mlb.com/api/v1/people/676571/stats?stats=gameLog&group=pitching&season=2019&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:57.841099Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:58.064997Z"}]`. Manifest query ID `c42229e15b502864f7054416d1c67802549a05c0cf360b23790903ca49d86223`.

<a id="q-526cc03f5af1f8c0140dd2e37e420284f246d82bb1ca13c2b9f631aa0a196c76"></a>

FACT: Q-526cc03f5af1: [GET people/676571/stats](https://statsapi.mlb.com/api/v1/people/676571/stats?stats=season&group=pitching&season=2019&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2019, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:58.150387Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:58.565105Z"}]`. Manifest query ID `526cc03f5af1f8c0140dd2e37e420284f246d82bb1ca13c2b9f631aa0a196c76`.

<a id="q-415d9ba3bbffd8c2fd631f1c54cdb2935223048911e15f015fa1eb24b3ff1292"></a>

FACT: Q-415d9ba3bbff: [GET people/676571/stats](https://statsapi.mlb.com/api/v1/people/676571/stats?stats=gameLog&group=pitching&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-05-31); parameters=`{"endDate": "2019-05-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:58.645131Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:59.065230Z"}]`. Manifest query ID `415d9ba3bbffd8c2fd631f1c54cdb2935223048911e15f015fa1eb24b3ff1292`.

<a id="q-d2e1a14b62ab3a1e10f8def706a75e263c1bda450b3ae989a43895925fa0ba72"></a>

FACT: Q-d2e1a14b62ab: [GET people/676571/stats](https://statsapi.mlb.com/api/v1/people/676571/stats?stats=gameLog&group=pitching&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-07-15); parameters=`{"endDate": "2019-07-15", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:59.153195Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:36:59.565302Z"}]`. Manifest query ID `d2e1a14b62ab3a1e10f8def706a75e263c1bda450b3ae989a43895925fa0ba72`.

<a id="q-258a055c7d6ddc0bff1718a2ccaa1dd40f0fe6fed13902b9ceadd921728a7466"></a>

FACT: Q-258a055c7d6d: [GET people/676571/stats](https://statsapi.mlb.com/api/v1/people/676571/stats?stats=gameLog&group=pitching&season=2019&sportIds=14&gameType=R&startDate=2019-01-01&endDate=2019-08-31); parameters=`{"endDate": "2019-08-31", "gameType": "R", "group": "pitching", "season": 2019, "sportIds": 14, "startDate": "2019-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:36:59.637722Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:00.065381Z"}]`. Manifest query ID `258a055c7d6ddc0bff1718a2ccaa1dd40f0fe6fed13902b9ceadd921728a7466`.

<a id="q-cdf39135f546e07a8c4b931aa38e2c8bbf56863d18478927746d05d96474557b"></a>

FACT: Q-cdf39135f546: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2021&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:00.143754Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:00.565455Z"}]`. Manifest query ID `cdf39135f546e07a8c4b931aa38e2c8bbf56863d18478927746d05d96474557b`.

<a id="q-2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8"></a>

FACT: Q-2532063af6d0: [GET people/669742/stats](https://statsapi.mlb.com/api/v1/people/669742/stats?stats=gameLog&group=hitting&season=2021&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:00.754973Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:01.065529Z"}]`. Manifest query ID `2532063af6d0497dccf050dfc33ebbd67b7e1ee369994f551f83a7acb3c4dec8`.

<a id="q-a226fa376e40273ab2e84844a3ec8ed5eec03488249794d2cbb8da685fc91c0b"></a>

FACT: Q-a226fa376e40: [GET people/669742/stats](https://statsapi.mlb.com/api/v1/people/669742/stats?stats=season&group=hitting&season=2021&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:01.180875Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:01.565608Z"}]`. Manifest query ID `a226fa376e40273ab2e84844a3ec8ed5eec03488249794d2cbb8da685fc91c0b`.

<a id="q-9a54c9c0a290a29570688dfcbd645c710482f382a29b26b815317cf4d65880f8"></a>

FACT: Q-9a54c9c0a290: [GET people/669742/stats](https://statsapi.mlb.com/api/v1/people/669742/stats?stats=gameLog&group=hitting&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:01.630405Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:02.065685Z"}]`. Manifest query ID `9a54c9c0a290a29570688dfcbd645c710482f382a29b26b815317cf4d65880f8`.

<a id="q-a4b90a32427f0cae0abfc5cc73d855756947344f093d0e2b3a99a6f13d81e71e"></a>

FACT: Q-a4b90a32427f: [GET people/669742/stats](https://statsapi.mlb.com/api/v1/people/669742/stats?stats=gameLog&group=hitting&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:02.135642Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:02.565721Z"}]`. Manifest query ID `a4b90a32427f0cae0abfc5cc73d855756947344f093d0e2b3a99a6f13d81e71e`.

<a id="q-f6cc9c9b2d8bdf8a7edbc6b222fc16b2f2455ab6fbfe860b1a7991d8784b840d"></a>

FACT: Q-f6cc9c9b2d8b: [GET people/669742/stats](https://statsapi.mlb.com/api/v1/people/669742/stats?stats=gameLog&group=hitting&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:02.639317Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:03.065811Z"}]`. Manifest query ID `f6cc9c9b2d8bdf8a7edbc6b222fc16b2f2455ab6fbfe860b1a7991d8784b840d`.

<a id="q-406e4b77f3f3b0d80aa3fa9f08896508f19dff8caf227a151b2c70e0f77da746"></a>

FACT: Q-406e4b77f3f3: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2021&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:03.134471Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:03.565897Z"}]`. Manifest query ID `406e4b77f3f3b0d80aa3fa9f08896508f19dff8caf227a151b2c70e0f77da746`.

<a id="q-c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d"></a>

FACT: Q-c1e00c445379: [GET people/670426/stats](https://statsapi.mlb.com/api/v1/people/670426/stats?stats=gameLog&group=pitching&season=2021&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:03.736913Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:04.065977Z"}]`. Manifest query ID `c1e00c44537988b6d96f33dc3f8e2f8c3dd7a079a2ad1070e1ce021443b8427d`.

<a id="q-671352b42afbaf5ee9470b4f2b8e67ff8c307c05b82b901aa929196458d54227"></a>

FACT: Q-671352b42afb: [GET people/670426/stats](https://statsapi.mlb.com/api/v1/people/670426/stats?stats=season&group=pitching&season=2021&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:04.150040Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:04.566063Z"}]`. Manifest query ID `671352b42afbaf5ee9470b4f2b8e67ff8c307c05b82b901aa929196458d54227`.

<a id="q-478e94c7a1bc3ffe2e86d7205edb50dfdeb6d52dd22d0dfcdc306085c2381cd7"></a>

FACT: Q-478e94c7a1bc: [GET people/670426/stats](https://statsapi.mlb.com/api/v1/people/670426/stats?stats=gameLog&group=pitching&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:04.641589Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:05.066138Z"}]`. Manifest query ID `478e94c7a1bc3ffe2e86d7205edb50dfdeb6d52dd22d0dfcdc306085c2381cd7`.

<a id="q-3b31ed05974676f4c1a613da4023b4f5435471bec054bf2dbb795ebf8aa4a4b4"></a>

FACT: Q-3b31ed059746: [GET people/670426/stats](https://statsapi.mlb.com/api/v1/people/670426/stats?stats=gameLog&group=pitching&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:05.146268Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:05.566212Z"}]`. Manifest query ID `3b31ed05974676f4c1a613da4023b4f5435471bec054bf2dbb795ebf8aa4a4b4`.

<a id="q-f1b9fed12525224a2c122cac00d9ef6f471de09338932626f8621edaf30289bd"></a>

FACT: Q-f1b9fed12525: [GET people/670426/stats](https://statsapi.mlb.com/api/v1/people/670426/stats?stats=gameLog&group=pitching&season=2021&sportIds=11&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 11, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:05.649830Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:06.066289Z"}]`. Manifest query ID `f1b9fed12525224a2c122cac00d9ef6f471de09338932626f8621edaf30289bd`.

<a id="q-2c8fc5d909c2da3fa627ff047cb21d8a33ec8118f045d1a1a40b8de5d97969ef"></a>

FACT: Q-2c8fc5d909c2: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2021&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:06.132485Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:06.566367Z"}]`. Manifest query ID `2c8fc5d909c2da3fa627ff047cb21d8a33ec8118f045d1a1a40b8de5d97969ef`.

<a id="q-9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac"></a>

FACT: Q-9bcc720db2fe: [GET people/669722/stats](https://statsapi.mlb.com/api/v1/people/669722/stats?stats=gameLog&group=hitting&season=2021&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:06.723724Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:07.066440Z"}]`. Manifest query ID `9bcc720db2fe90e2e57711a99eba450b3a2fdaf097562842f8da4564c3090dac`.

<a id="q-cc00710c2dd99ec1cc66e97f09957df5b2ca5445160a3233cde3377fc8b6883e"></a>

FACT: Q-cc00710c2dd9: [GET people/669722/stats](https://statsapi.mlb.com/api/v1/people/669722/stats?stats=season&group=hitting&season=2021&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:07.137239Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:07.566520Z"}]`. Manifest query ID `cc00710c2dd99ec1cc66e97f09957df5b2ca5445160a3233cde3377fc8b6883e`.

<a id="q-7c3d9c919d71b4e54422d714688adef9a65be3a694781681cbb20e1b25313ff0"></a>

FACT: Q-7c3d9c919d71: [GET people/669722/stats](https://statsapi.mlb.com/api/v1/people/669722/stats?stats=gameLog&group=hitting&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:07.631197Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:08.066636Z"}]`. Manifest query ID `7c3d9c919d71b4e54422d714688adef9a65be3a694781681cbb20e1b25313ff0`.

<a id="q-377e87ab65fb5a41cfe52bebe74b961cd51af0d6f61ae8f89f5e79f984f28859"></a>

FACT: Q-377e87ab65fb: [GET people/669722/stats](https://statsapi.mlb.com/api/v1/people/669722/stats?stats=gameLog&group=hitting&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:08.140111Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:08.566723Z"}]`. Manifest query ID `377e87ab65fb5a41cfe52bebe74b961cd51af0d6f61ae8f89f5e79f984f28859`.

<a id="q-68a91646fbf0af346597c9d69b83511c8d99ecc3ee4a6df773b7c524221ceca9"></a>

FACT: Q-68a91646fbf0: [GET people/669722/stats](https://statsapi.mlb.com/api/v1/people/669722/stats?stats=gameLog&group=hitting&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:08.632179Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:09.066802Z"}]`. Manifest query ID `68a91646fbf0af346597c9d69b83511c8d99ecc3ee4a6df773b7c524221ceca9`.

<a id="q-022da2494bf982451531f8f22238a0df02eecaab03ed54d66e5d2b71caa32dcc"></a>

FACT: Q-022da2494bf9: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2021&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:09.138747Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:09.566878Z"}]`. Manifest query ID `022da2494bf982451531f8f22238a0df02eecaab03ed54d66e5d2b71caa32dcc`.

<a id="q-319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9"></a>

FACT: Q-319b9e835ec9: [GET people/646243/stats](https://statsapi.mlb.com/api/v1/people/646243/stats?stats=gameLog&group=pitching&season=2021&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:09.708734Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:10.067037Z"}]`. Manifest query ID `319b9e835ec9d02fd3891de1abe1c2e9140fdbc2a3708f0d02d8ba792b0bd5b9`.

<a id="q-d34da7243fa64c26868c1f7585e4602c996177b8ca6ca29934be5a7e4c83c4a4"></a>

FACT: Q-d34da7243fa6: [GET people/646243/stats](https://statsapi.mlb.com/api/v1/people/646243/stats?stats=season&group=pitching&season=2021&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:10.134444Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:10.567110Z"}]`. Manifest query ID `d34da7243fa64c26868c1f7585e4602c996177b8ca6ca29934be5a7e4c83c4a4`.

<a id="q-61b57f5cd5051dcd29104915dd2b628e8b8ea89a458117c6f796617033c05e80"></a>

FACT: Q-61b57f5cd505: [GET people/646243/stats](https://statsapi.mlb.com/api/v1/people/646243/stats?stats=gameLog&group=pitching&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:10.644530Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:11.067189Z"}]`. Manifest query ID `61b57f5cd5051dcd29104915dd2b628e8b8ea89a458117c6f796617033c05e80`.

<a id="q-67d59feda549b8743c53e9a36854a09629b848a1ea084e8e8056d476fdb876f2"></a>

FACT: Q-67d59feda549: [GET people/646243/stats](https://statsapi.mlb.com/api/v1/people/646243/stats?stats=gameLog&group=pitching&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:11.134642Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:11.567263Z"}]`. Manifest query ID `67d59feda549b8743c53e9a36854a09629b848a1ea084e8e8056d476fdb876f2`.

<a id="q-18bf5ae6e9cb1477fff6ae5aa16621d865e661dfd45aab6c3a8884160e8cab4c"></a>

FACT: Q-18bf5ae6e9cb: [GET people/646243/stats](https://statsapi.mlb.com/api/v1/people/646243/stats?stats=gameLog&group=pitching&season=2021&sportIds=12&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 12, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:11.633715Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:12.067351Z"}]`. Manifest query ID `18bf5ae6e9cb1477fff6ae5aa16621d865e661dfd45aab6c3a8884160e8cab4c`.

<a id="q-b7b6b78822bd1c45c0558455dc4c5a35b6e1ea02ec9727bf9a8d1feddcc238db"></a>

FACT: Q-b7b6b78822bd: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2021&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:12.136666Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:12.567433Z"}]`. Manifest query ID `b7b6b78822bd1c45c0558455dc4c5a35b6e1ea02ec9727bf9a8d1feddcc238db`.

<a id="q-0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1"></a>

FACT: Q-0045a0ad6f48: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2021&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:12.682952Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:13.067509Z"}]`. Manifest query ID `0045a0ad6f48065f920f9b2f79cb373d925d118159bae0cad5391497ddc297e1`.

<a id="q-b42d3de143fd519725d5b580f35a425d79df4a0ba533986d261f32070f5cc474"></a>

FACT: Q-b42d3de143fd: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=season&group=hitting&season=2021&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:13.133797Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:13.567582Z"}]`. Manifest query ID `b42d3de143fd519725d5b580f35a425d79df4a0ba533986d261f32070f5cc474`.

<a id="q-b6f8c8203ead8cdf1a01122d4644a20a94d82723616bfc6920da9e7f87941331"></a>

FACT: Q-b6f8c8203ead: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:13.631635Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:14.067668Z"}]`. Manifest query ID `b6f8c8203ead8cdf1a01122d4644a20a94d82723616bfc6920da9e7f87941331`.

<a id="q-4220bfeeb6ee06918201f4e7ee78c2edc0ea283975caad740953fc7452f084d6"></a>

FACT: Q-4220bfeeb6ee: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:14.137430Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:14.567771Z"}]`. Manifest query ID `4220bfeeb6ee06918201f4e7ee78c2edc0ea283975caad740953fc7452f084d6`.

<a id="q-69d47fd2ebb67ac33c816b0a74c823ad69a45ae33e69a4f51187c13ff1966cfc"></a>

FACT: Q-69d47fd2ebb6: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:14.644712Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:15.067839Z"}]`. Manifest query ID `69d47fd2ebb67ac33c816b0a74c823ad69a45ae33e69a4f51187c13ff1966cfc`.

<a id="q-aaa6e073d5d8eaa735ed0f653948025d9345d648db14a87eed064699d06a6bd1"></a>

FACT: Q-aaa6e073d5d8: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2021&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:15.137066Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:15.567913Z"}]`. Manifest query ID `aaa6e073d5d8eaa735ed0f653948025d9345d648db14a87eed064699d06a6bd1`.

<a id="q-37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8"></a>

FACT: Q-37bf061513b4: [GET people/682121/stats](https://statsapi.mlb.com/api/v1/people/682121/stats?stats=gameLog&group=pitching&season=2021&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:15.742928Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:16.067985Z"}]`. Manifest query ID `37bf061513b4d8a84a215acd79e5f3bd2c5ddfbc22b8e690089c792a9af1afd8`.

<a id="q-51e59fe535136601f7e1d2e475a8401783adfe521692f5311bf6ac2638502272"></a>

FACT: Q-51e59fe53513: [GET people/682121/stats](https://statsapi.mlb.com/api/v1/people/682121/stats?stats=season&group=pitching&season=2021&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:16.145576Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:16.568032Z"}]`. Manifest query ID `51e59fe535136601f7e1d2e475a8401783adfe521692f5311bf6ac2638502272`.

<a id="q-dcbc249799d5824be78fa407ebb18777fd62781ec7a90ab41ee903044558e95e"></a>

FACT: Q-dcbc249799d5: [GET people/682121/stats](https://statsapi.mlb.com/api/v1/people/682121/stats?stats=gameLog&group=pitching&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:16.638340Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:17.068113Z"}]`. Manifest query ID `dcbc249799d5824be78fa407ebb18777fd62781ec7a90ab41ee903044558e95e`.

<a id="q-ebe6346cf5584e3b9c2f1b7ad88e4d04f5fa4f21bdc3a4a948513a135085b727"></a>

FACT: Q-ebe6346cf558: [GET people/682121/stats](https://statsapi.mlb.com/api/v1/people/682121/stats?stats=gameLog&group=pitching&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:17.133734Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:17.568184Z"}]`. Manifest query ID `ebe6346cf5584e3b9c2f1b7ad88e4d04f5fa4f21bdc3a4a948513a135085b727`.

<a id="q-abb11eaae5cb06f9b8d78724051fa75a9273bc0520b07ae1cb7eedbe082e7930"></a>

FACT: Q-abb11eaae5cb: [GET people/682121/stats](https://statsapi.mlb.com/api/v1/people/682121/stats?stats=gameLog&group=pitching&season=2021&sportIds=13&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 13, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:17.635437Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:18.068265Z"}]`. Manifest query ID `abb11eaae5cb06f9b8d78724051fa75a9273bc0520b07ae1cb7eedbe082e7930`.

<a id="q-14468c069f6978dbbed3c02024d2eebc87da15880b0cf7bdebeab41757aa2c48"></a>

FACT: Q-14468c069f69: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2021&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:18.132793Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:18.568343Z"}]`. Manifest query ID `14468c069f6978dbbed3c02024d2eebc87da15880b0cf7bdebeab41757aa2c48`.

<a id="q-3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5"></a>

FACT: Q-3678f350e868: [GET people/682868/stats](https://statsapi.mlb.com/api/v1/people/682868/stats?stats=gameLog&group=hitting&season=2021&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:18.697967Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:19.068427Z"}]`. Manifest query ID `3678f350e8684cdb3cc46e2bda8b4c83c7621e56842f1794c051b086db7eafb5`.

<a id="q-5d2867eefc2a7ed353c31f1823ca947cc1abe7efd1eb0463b0a03fbf918163bd"></a>

FACT: Q-5d2867eefc2a: [GET people/682868/stats](https://statsapi.mlb.com/api/v1/people/682868/stats?stats=season&group=hitting&season=2021&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2021, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:19.138076Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:19.568498Z"}]`. Manifest query ID `5d2867eefc2a7ed353c31f1823ca947cc1abe7efd1eb0463b0a03fbf918163bd`.

<a id="q-5788d5d1a1b9147a62286999b42aa26376b48e84378d5fab61ee59b362911918"></a>

FACT: Q-5788d5d1a1b9: [GET people/682868/stats](https://statsapi.mlb.com/api/v1/people/682868/stats?stats=gameLog&group=hitting&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:19.651199Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:20.068526Z"}]`. Manifest query ID `5788d5d1a1b9147a62286999b42aa26376b48e84378d5fab61ee59b362911918`.

<a id="q-28b4198e0146460a8512c731bdb3f375db62cdfd3659e19d316150cfca643ccc"></a>

FACT: Q-28b4198e0146: [GET people/682868/stats](https://statsapi.mlb.com/api/v1/people/682868/stats?stats=gameLog&group=hitting&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:20.139579Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:20.568604Z"}]`. Manifest query ID `28b4198e0146460a8512c731bdb3f375db62cdfd3659e19d316150cfca643ccc`.

<a id="q-3bb103e2f0cfd2ecbd72351de35f5c1efbe7b97ac97eda4a572cf92e1fc1c411"></a>

FACT: Q-3bb103e2f0cf: [GET people/682868/stats](https://statsapi.mlb.com/api/v1/people/682868/stats?stats=gameLog&group=hitting&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "hitting", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:20.637991Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:21.068677Z"}]`. Manifest query ID `3bb103e2f0cfd2ecbd72351de35f5c1efbe7b97ac97eda4a572cf92e1fc1c411`.

<a id="q-771ff2437fcd62766c5ed19c64ef7c6e48bd54d4a305c70ac56a237a94585dd3"></a>

FACT: Q-771ff2437fcd: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2021&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2021, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:21.139808Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:21.568750Z"}]`. Manifest query ID `771ff2437fcd62766c5ed19c64ef7c6e48bd54d4a305c70ac56a237a94585dd3`.

<a id="q-67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2"></a>

FACT: Q-67aa90d57e81: [GET people/675848/stats](https://statsapi.mlb.com/api/v1/people/675848/stats?stats=gameLog&group=pitching&season=2021&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:21.744723Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:22.068827Z"}]`. Manifest query ID `67aa90d57e814deb18fae4724eee007809cf689b2ed3f11fca48bc580a34b9d2`.

<a id="q-403466caab91d1ffe566e5a5ad461a67ae01113095975a946b94c5b7133a54d8"></a>

FACT: Q-403466caab91: [GET people/675848/stats](https://statsapi.mlb.com/api/v1/people/675848/stats?stats=season&group=pitching&season=2021&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2021, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:22.145680Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:22.568902Z"}]`. Manifest query ID `403466caab91d1ffe566e5a5ad461a67ae01113095975a946b94c5b7133a54d8`.

<a id="q-5f7cba1e248d81ca700f56f4d407a5bf34695969195e64fcf31556a33226dc41"></a>

FACT: Q-5f7cba1e248d: [GET people/675848/stats](https://statsapi.mlb.com/api/v1/people/675848/stats?stats=gameLog&group=pitching&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-05-31); parameters=`{"endDate": "2021-05-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:22.648188Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:23.068983Z"}]`. Manifest query ID `5f7cba1e248d81ca700f56f4d407a5bf34695969195e64fcf31556a33226dc41`.

<a id="q-6ca0b3d3f81b9ff2306ea613f4b5da79d7de871aba2016f9500b63a0d42b5253"></a>

FACT: Q-6ca0b3d3f81b: [GET people/675848/stats](https://statsapi.mlb.com/api/v1/people/675848/stats?stats=gameLog&group=pitching&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-07-15); parameters=`{"endDate": "2021-07-15", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:23.141991Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:23.569042Z"}]`. Manifest query ID `6ca0b3d3f81b9ff2306ea613f4b5da79d7de871aba2016f9500b63a0d42b5253`.

<a id="q-e4ed93557d007c072f4f5ca632e05e1748f5c1dad4daae0d374c1ae4434b774d"></a>

FACT: Q-e4ed93557d00: [GET people/675848/stats](https://statsapi.mlb.com/api/v1/people/675848/stats?stats=gameLog&group=pitching&season=2021&sportIds=14&gameType=R&startDate=2021-01-01&endDate=2021-08-31); parameters=`{"endDate": "2021-08-31", "gameType": "R", "group": "pitching", "season": 2021, "sportIds": 14, "startDate": "2021-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:23.636135Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:24.069122Z"}]`. Manifest query ID `e4ed93557d007c072f4f5ca632e05e1748f5c1dad4daae0d374c1ae4434b774d`.

<a id="q-8273e8ce1e9beb438dbe0da8fb45efb8651d0f8c19b98a41ebed269cef0d1ec8"></a>

FACT: Q-8273e8ce1e9b: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2022&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:24.139330Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:24.569196Z"}]`. Manifest query ID `8273e8ce1e9beb438dbe0da8fb45efb8651d0f8c19b98a41ebed269cef0d1ec8`.

<a id="q-38be493cd5196c8a5d81e1d6ced9a67b7a87bf5c5553cac379811467b4271ad6"></a>

FACT: Q-38be493cd519: [GET people/623507/stats](https://statsapi.mlb.com/api/v1/people/623507/stats?stats=gameLog&group=hitting&season=2022&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:24.711223Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:25.069275Z"}]`. Manifest query ID `38be493cd5196c8a5d81e1d6ced9a67b7a87bf5c5553cac379811467b4271ad6`.

<a id="q-abbb56bb32f8f25530aae197ac6fd04dd90941b16274b2076841c556192278d9"></a>

FACT: Q-abbb56bb32f8: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2022&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:25.160981Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:25.569346Z"}]`. Manifest query ID `abbb56bb32f8f25530aae197ac6fd04dd90941b16274b2076841c556192278d9`.

<a id="q-a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8"></a>

FACT: Q-a2689b712109: [GET people/545346/stats](https://statsapi.mlb.com/api/v1/people/545346/stats?stats=gameLog&group=pitching&season=2022&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:25.759432Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:26.069420Z"}]`. Manifest query ID `a2689b7121090a8192f4ac449562a82edcc37720ef294b7ef0524c0a887bf3e8`.

<a id="q-1656f71c605ef51388895c8dcd78798ce066adaec816a7b831041e96cea98274"></a>

FACT: Q-1656f71c605e: [GET people/545346/stats](https://statsapi.mlb.com/api/v1/people/545346/stats?stats=season&group=pitching&season=2022&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:26.145453Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:26.569492Z"}]`. Manifest query ID `1656f71c605ef51388895c8dcd78798ce066adaec816a7b831041e96cea98274`.

<a id="q-3506add38b9885503effc43e22a52a194da855a4a89ff4cc36937626e9d4fe30"></a>

FACT: Q-3506add38b98: [GET people/545346/stats](https://statsapi.mlb.com/api/v1/people/545346/stats?stats=gameLog&group=pitching&season=2022&sportIds=11&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 11, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:26.644351Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:27.069569Z"}]`. Manifest query ID `3506add38b9885503effc43e22a52a194da855a4a89ff4cc36937626e9d4fe30`.

<a id="q-fee580187d68fa2169afdc4c363974d75def203109b706c67a01518ea612fca5"></a>

FACT: Q-fee580187d68: [GET people/545346/stats](https://statsapi.mlb.com/api/v1/people/545346/stats?stats=gameLog&group=pitching&season=2022&sportIds=11&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 11, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:27.131740Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:27.569645Z"}]`. Manifest query ID `fee580187d68fa2169afdc4c363974d75def203109b706c67a01518ea612fca5`.

<a id="q-770932cdf8629738f1452494447f2a360cfebdbf0ce26d3c1df76fe23b3eb77a"></a>

FACT: Q-770932cdf862: [GET people/545346/stats](https://statsapi.mlb.com/api/v1/people/545346/stats?stats=gameLog&group=pitching&season=2022&sportIds=11&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 11, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:27.646619Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:28.069722Z"}]`. Manifest query ID `770932cdf8629738f1452494447f2a360cfebdbf0ce26d3c1df76fe23b3eb77a`.

<a id="q-6bbd96da19c5d056cf3bfb21b31e8e88f7a0a8a07441309292cf0c9e3481cc1b"></a>

FACT: Q-6bbd96da19c5: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2022&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:28.137341Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:28.569798Z"}]`. Manifest query ID `6bbd96da19c5d056cf3bfb21b31e8e88f7a0a8a07441309292cf0c9e3481cc1b`.

<a id="q-3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a"></a>

FACT: Q-3cb1d41cd2cb: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2022&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:28.702783Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:29.069921Z"}]`. Manifest query ID `3cb1d41cd2cbf9da854afaf13e6ddc7263623a1cee44be65d9a8c000e188857a`.

<a id="q-2388ccb4b4c1e480c7d052d626601318e8779c40b2bddb2298e7ebce649c5815"></a>

FACT: Q-2388ccb4b4c1: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=season&group=hitting&season=2022&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:29.141872Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:29.570017Z"}]`. Manifest query ID `2388ccb4b4c1e480c7d052d626601318e8779c40b2bddb2298e7ebce649c5815`.

<a id="q-7148278dd7390f0e2f3e5759afe7c01fecdcd0d048252fc4f0d8c3a65741d763"></a>

FACT: Q-7148278dd739: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:29.635886Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:30.070040Z"}]`. Manifest query ID `7148278dd7390f0e2f3e5759afe7c01fecdcd0d048252fc4f0d8c3a65741d763`.

<a id="q-da16dba2e740aa1f05bfd73a29f382a74e3958b1019359c8706db2df47c968c3"></a>

FACT: Q-da16dba2e740: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:30.137264Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:30.570117Z"}]`. Manifest query ID `da16dba2e740aa1f05bfd73a29f382a74e3958b1019359c8706db2df47c968c3`.

<a id="q-d92a1b1da7eec4c79c33043cf4548df5f8059c42701f6e9fd05ce7f183e59f19"></a>

FACT: Q-d92a1b1da7ee: [GET people/681624/stats](https://statsapi.mlb.com/api/v1/people/681624/stats?stats=gameLog&group=hitting&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:30.645594Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:31.070195Z"}]`. Manifest query ID `d92a1b1da7eec4c79c33043cf4548df5f8059c42701f6e9fd05ce7f183e59f19`.

<a id="q-e684cfc6a4af65fa0aa79c913bda677990738b49f9e0e6b30d1256600b42b287"></a>

FACT: Q-e684cfc6a4af: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2022&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:31.139717Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:31.570273Z"}]`. Manifest query ID `e684cfc6a4af65fa0aa79c913bda677990738b49f9e0e6b30d1256600b42b287`.

<a id="q-5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b"></a>

FACT: Q-5ffc38ef51dd: [GET people/688427/stats](https://statsapi.mlb.com/api/v1/people/688427/stats?stats=gameLog&group=pitching&season=2022&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:31.730578Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:32.070360Z"}]`. Manifest query ID `5ffc38ef51dd156c197e702f9b26b93c815b458b698e4204ef9aa983c7e1876b`.

<a id="q-576bf4631da27741a03fdf31e21ea886c77f5783b962533b978b4ecb696dcb14"></a>

FACT: Q-576bf4631da2: [GET people/688427/stats](https://statsapi.mlb.com/api/v1/people/688427/stats?stats=season&group=pitching&season=2022&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:32.138184Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:32.570432Z"}]`. Manifest query ID `576bf4631da27741a03fdf31e21ea886c77f5783b962533b978b4ecb696dcb14`.

<a id="q-8420a0788b6789ec6729da7ae897fb8f14372f9c617c64ae9547144896fe6d0b"></a>

FACT: Q-8420a0788b67: [GET people/688427/stats](https://statsapi.mlb.com/api/v1/people/688427/stats?stats=gameLog&group=pitching&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:32.651431Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:33.070522Z"}]`. Manifest query ID `8420a0788b6789ec6729da7ae897fb8f14372f9c617c64ae9547144896fe6d0b`.

<a id="q-07b093532b851023eab41205c8243ddfc0f91f019bcbb609116e3e066a0b3a5f"></a>

FACT: Q-07b093532b85: [GET people/688427/stats](https://statsapi.mlb.com/api/v1/people/688427/stats?stats=gameLog&group=pitching&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:33.138369Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:33.570603Z"}]`. Manifest query ID `07b093532b851023eab41205c8243ddfc0f91f019bcbb609116e3e066a0b3a5f`.

<a id="q-828088ba669affc85a569ec96f6d122330c44bd424a00e02d67c7cfc97375133"></a>

FACT: Q-828088ba669a: [GET people/688427/stats](https://statsapi.mlb.com/api/v1/people/688427/stats?stats=gameLog&group=pitching&season=2022&sportIds=12&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 12, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:33.639228Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:34.070679Z"}]`. Manifest query ID `828088ba669affc85a569ec96f6d122330c44bd424a00e02d67c7cfc97375133`.

<a id="q-1ddf27343009089aa00bad50de553d4aac5fab52fd0cfc8806d75b6bbdea5763"></a>

FACT: Q-1ddf27343009: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2022&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:34.145046Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:34.570765Z"}]`. Manifest query ID `1ddf27343009089aa00bad50de553d4aac5fab52fd0cfc8806d75b6bbdea5763`.

<a id="q-630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83"></a>

FACT: Q-630493a50098: [GET people/678391/stats](https://statsapi.mlb.com/api/v1/people/678391/stats?stats=gameLog&group=hitting&season=2022&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:34.701738Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:35.070834Z"}]`. Manifest query ID `630493a50098c879db361dc46673cbc5f28b51c73570ce11b0b0b9f2d88f9e83`.

<a id="q-0ff9fbbac1fb1006edf19d15c7ff9ac36aac22422d420cd4f2e105e7cf48acf9"></a>

FACT: Q-0ff9fbbac1fb: [GET people/678391/stats](https://statsapi.mlb.com/api/v1/people/678391/stats?stats=season&group=hitting&season=2022&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:35.132437Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:35.570908Z"}]`. Manifest query ID `0ff9fbbac1fb1006edf19d15c7ff9ac36aac22422d420cd4f2e105e7cf48acf9`.

<a id="q-0f3bc5a534542b8067f687cd06f8a38b934d96f7ab1c45d6225a6e0082a282c6"></a>

FACT: Q-0f3bc5a53454: [GET people/678391/stats](https://statsapi.mlb.com/api/v1/people/678391/stats?stats=gameLog&group=hitting&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:35.640297Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:36.070987Z"}]`. Manifest query ID `0f3bc5a534542b8067f687cd06f8a38b934d96f7ab1c45d6225a6e0082a282c6`.

<a id="q-aaf48bde1efb8542167b657220a6951a7c67a31a859e9c5fb813c38d52059756"></a>

FACT: Q-aaf48bde1efb: [GET people/678391/stats](https://statsapi.mlb.com/api/v1/people/678391/stats?stats=gameLog&group=hitting&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:36.140396Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:36.571031Z"}]`. Manifest query ID `aaf48bde1efb8542167b657220a6951a7c67a31a859e9c5fb813c38d52059756`.

<a id="q-86b550ef80efc8f3331564e7a640949ab127d120955dabaddf4706794eff63e5"></a>

FACT: Q-86b550ef80ef: [GET people/678391/stats](https://statsapi.mlb.com/api/v1/people/678391/stats?stats=gameLog&group=hitting&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:36.642405Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:37.071106Z"}]`. Manifest query ID `86b550ef80efc8f3331564e7a640949ab127d120955dabaddf4706794eff63e5`.

<a id="q-6a525bcc25ab979a2e4a00e39dd1f293bbd9afc0c2c9adf34d902398406dde9f"></a>

FACT: Q-6a525bcc25ab: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2022&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:37.137820Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:37.571180Z"}]`. Manifest query ID `6a525bcc25ab979a2e4a00e39dd1f293bbd9afc0c2c9adf34d902398406dde9f`.

<a id="q-e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55"></a>

FACT: Q-e87402d7a1c3: [GET people/687841/stats](https://statsapi.mlb.com/api/v1/people/687841/stats?stats=gameLog&group=pitching&season=2022&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:37.719155Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:38.071259Z"}]`. Manifest query ID `e87402d7a1c3d93ddbf1887de5f1bdbcb3b80a31bcc3187c6bc2f1928a2b2f55`.

<a id="q-7f2e72894aa970ec8f8bc38b70568c35518f91dbc77ae8ecbfd3bc3d65971ea6"></a>

FACT: Q-7f2e72894aa9: [GET people/687841/stats](https://statsapi.mlb.com/api/v1/people/687841/stats?stats=season&group=pitching&season=2022&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:38.162558Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:38.571332Z"}]`. Manifest query ID `7f2e72894aa970ec8f8bc38b70568c35518f91dbc77ae8ecbfd3bc3d65971ea6`.

<a id="q-f94ba1a1004e07606ea3d035bb968699ed7cefbb2e5d960a5651b342e26d27c0"></a>

FACT: Q-f94ba1a1004e: [GET people/687841/stats](https://statsapi.mlb.com/api/v1/people/687841/stats?stats=gameLog&group=pitching&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:38.645815Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:39.071405Z"}]`. Manifest query ID `f94ba1a1004e07606ea3d035bb968699ed7cefbb2e5d960a5651b342e26d27c0`.

<a id="q-826949136b9b06d2bf26c3626ec15c6be5ed1bea83142892da1af852e0ad47e9"></a>

FACT: Q-826949136b9b: [GET people/687841/stats](https://statsapi.mlb.com/api/v1/people/687841/stats?stats=gameLog&group=pitching&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:39.135086Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:39.571483Z"}]`. Manifest query ID `826949136b9b06d2bf26c3626ec15c6be5ed1bea83142892da1af852e0ad47e9`.

<a id="q-9996380e6659677c675b10a37632890a77dcaf9db364dab20d174e50d84f9163"></a>

FACT: Q-9996380e6659: [GET people/687841/stats](https://statsapi.mlb.com/api/v1/people/687841/stats?stats=gameLog&group=pitching&season=2022&sportIds=13&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 13, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:39.642658Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:40.071563Z"}]`. Manifest query ID `9996380e6659677c675b10a37632890a77dcaf9db364dab20d174e50d84f9163`.

<a id="q-e34ca5abe565cce598398f66310b002cf9104e9dca3bed902427e18a040880c2"></a>

FACT: Q-e34ca5abe565: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2022&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:40.142328Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:40.571634Z"}]`. Manifest query ID `e34ca5abe565cce598398f66310b002cf9104e9dca3bed902427e18a040880c2`.

<a id="q-5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa"></a>

FACT: Q-5da751e70d49: [GET people/692348/stats](https://statsapi.mlb.com/api/v1/people/692348/stats?stats=gameLog&group=hitting&season=2022&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:40.708152Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:41.071709Z"}]`. Manifest query ID `5da751e70d4928258c7a559f70ef3e3bc50e0707cca01d13d965f40e4063b0aa`.

<a id="q-9fa15951a75874e3d70b741d452fa2a2d1e3993fe6a57abb6bd906421c587126"></a>

FACT: Q-9fa15951a758: [GET people/692348/stats](https://statsapi.mlb.com/api/v1/people/692348/stats?stats=season&group=hitting&season=2022&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2022, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:41.167103Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:41.571782Z"}]`. Manifest query ID `9fa15951a75874e3d70b741d452fa2a2d1e3993fe6a57abb6bd906421c587126`.

<a id="q-1895eecd023896174d0c8424ca860c5eed1d3fd9c9e5bc365b486a25f203cad5"></a>

FACT: Q-1895eecd0238: [GET people/692348/stats](https://statsapi.mlb.com/api/v1/people/692348/stats?stats=gameLog&group=hitting&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:41.655407Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:42.071856Z"}]`. Manifest query ID `1895eecd023896174d0c8424ca860c5eed1d3fd9c9e5bc365b486a25f203cad5`.

<a id="q-f35524e664842e9d0060bdc4816dcce943b5d9cdbe112c3f6d39f153a84784f8"></a>

FACT: Q-f35524e66484: [GET people/692348/stats](https://statsapi.mlb.com/api/v1/people/692348/stats?stats=gameLog&group=hitting&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:42.139999Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:42.571930Z"}]`. Manifest query ID `f35524e664842e9d0060bdc4816dcce943b5d9cdbe112c3f6d39f153a84784f8`.

<a id="q-1e7ceb892dc2ebe528e4fc057774d4a3294f55d2ba44e00f07d775727b1e09b6"></a>

FACT: Q-1e7ceb892dc2: [GET people/692348/stats](https://statsapi.mlb.com/api/v1/people/692348/stats?stats=gameLog&group=hitting&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "hitting", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:42.640198Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:43.072012Z"}]`. Manifest query ID `1e7ceb892dc2ebe528e4fc057774d4a3294f55d2ba44e00f07d775727b1e09b6`.

<a id="q-c7b74974eced82d0432d25e983daaf58c20aa552cf66b38137da09874beca85e"></a>

FACT: Q-c7b74974eced: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2022&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2022, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:43.141450Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:43.572038Z"}]`. Manifest query ID `c7b74974eced82d0432d25e983daaf58c20aa552cf66b38137da09874beca85e`.

<a id="q-e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a"></a>

FACT: Q-e9b5c019c9da: [GET people/683928/stats](https://statsapi.mlb.com/api/v1/people/683928/stats?stats=gameLog&group=pitching&season=2022&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:43.724258Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:44.072114Z"}]`. Manifest query ID `e9b5c019c9dabeb1c8beece13aa74b708ecc8b31e1096584430fd42deb341f9a`.

<a id="q-3e2a54f3f77fb4e55ea07aa0de9c5853c1b1ff07e54071479592b233efcd9b21"></a>

FACT: Q-3e2a54f3f77f: [GET people/683928/stats](https://statsapi.mlb.com/api/v1/people/683928/stats?stats=season&group=pitching&season=2022&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2022, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:44.153678Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:44.572190Z"}]`. Manifest query ID `3e2a54f3f77fb4e55ea07aa0de9c5853c1b1ff07e54071479592b233efcd9b21`.

<a id="q-3d53415bc6bd566f8725c41bc1d254ec163cdc021ce675592e7cd60ed395ac56"></a>

FACT: Q-3d53415bc6bd: [GET people/683928/stats](https://statsapi.mlb.com/api/v1/people/683928/stats?stats=gameLog&group=pitching&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-05-31); parameters=`{"endDate": "2022-05-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:44.650825Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:45.072266Z"}]`. Manifest query ID `3d53415bc6bd566f8725c41bc1d254ec163cdc021ce675592e7cd60ed395ac56`.

<a id="q-7dee20ee8c9f3e521c913aac4cef72ef1c74666411f46e05762ea40fc43b0ac8"></a>

FACT: Q-7dee20ee8c9f: [GET people/683928/stats](https://statsapi.mlb.com/api/v1/people/683928/stats?stats=gameLog&group=pitching&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-07-15); parameters=`{"endDate": "2022-07-15", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:45.143148Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:45.572343Z"}]`. Manifest query ID `7dee20ee8c9f3e521c913aac4cef72ef1c74666411f46e05762ea40fc43b0ac8`.

<a id="q-f0eaaded9d2a65612dfab5ed5c0654a82580436e1585a95c0cebe87fd8439392"></a>

FACT: Q-f0eaaded9d2a: [GET people/683928/stats](https://statsapi.mlb.com/api/v1/people/683928/stats?stats=gameLog&group=pitching&season=2022&sportIds=14&gameType=R&startDate=2022-01-01&endDate=2022-08-31); parameters=`{"endDate": "2022-08-31", "gameType": "R", "group": "pitching", "season": 2022, "sportIds": 14, "startDate": "2022-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:45.681174Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:46.072414Z"}]`. Manifest query ID `f0eaaded9d2a65612dfab5ed5c0654a82580436e1585a95c0cebe87fd8439392`.

<a id="q-c632a9d998407711ae2bfc9740d7253089ec5350d93ae72dfbaedb3923293a8f"></a>

FACT: Q-c632a9d99840: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2023&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:46.141188Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:46.572489Z"}]`. Manifest query ID `c632a9d998407711ae2bfc9740d7253089ec5350d93ae72dfbaedb3923293a8f`.

<a id="q-e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a"></a>

FACT: Q-e10fd3f756d5: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2023&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:46.789395Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:47.072565Z"}]`. Manifest query ID `e10fd3f756d53478e408aafcaceb51ae87bee07bb634a4a7b6d5977351aa120a`.

<a id="q-ef06abb34fb0b94162764a26b47dfbdaf9e86fda0a19c4b7c7f6c1f6dadb41d4"></a>

FACT: Q-ef06abb34fb0: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=season&group=hitting&season=2023&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:47.142446Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:47.572637Z"}]`. Manifest query ID `ef06abb34fb0b94162764a26b47dfbdaf9e86fda0a19c4b7c7f6c1f6dadb41d4`.

<a id="q-8fd8dd4d2ca06ebd882a8d591376d6239e6537edbb565eac43fa531f3a0b77d6"></a>

FACT: Q-8fd8dd4d2ca0: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:47.641974Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:48.072755Z"}]`. Manifest query ID `8fd8dd4d2ca06ebd882a8d591376d6239e6537edbb565eac43fa531f3a0b77d6`.

<a id="q-e150918998897eb66a0d2210f82fe8e5604c127a5946d96c32f86d43eda09283"></a>

FACT: Q-e15091899889: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:48.148250Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:48.572848Z"}]`. Manifest query ID `e150918998897eb66a0d2210f82fe8e5604c127a5946d96c32f86d43eda09283`.

<a id="q-4ff7a4accb5f7f868fee9a9b084e565c4c3f782d5ff960df6a2dd1c1f922e792"></a>

FACT: Q-4ff7a4accb5f: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:48.648670Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:49.072917Z"}]`. Manifest query ID `4ff7a4accb5f7f868fee9a9b084e565c4c3f782d5ff960df6a2dd1c1f922e792`.

<a id="q-9c37e9cb090b7c33a13c244681de374f6f48538c118c2556c64cc84257208292"></a>

FACT: Q-9c37e9cb090b: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2023&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:49.144654Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:49.573037Z"}]`. Manifest query ID `9c37e9cb090b7c33a13c244681de374f6f48538c118c2556c64cc84257208292`.

<a id="q-9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c"></a>

FACT: Q-9c024bff08a4: [GET people/685126/stats](https://statsapi.mlb.com/api/v1/people/685126/stats?stats=gameLog&group=pitching&season=2023&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:49.742261Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:50.073126Z"}]`. Manifest query ID `9c024bff08a48d13dde1b5266f719ff03ce6bf8038d2cd5b11407821e623c45c`.

<a id="q-bb33d73d5dcc8025e602c35605ca73e33b80dbaad83aa50cd1856a99edeb3269"></a>

FACT: Q-bb33d73d5dcc: [GET people/685126/stats](https://statsapi.mlb.com/api/v1/people/685126/stats?stats=season&group=pitching&season=2023&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:50.138037Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:50.573216Z"}]`. Manifest query ID `bb33d73d5dcc8025e602c35605ca73e33b80dbaad83aa50cd1856a99edeb3269`.

<a id="q-d93287baab285934e0567a99e7b0ec724608ca3255b09edd73e260c58761180c"></a>

FACT: Q-d93287baab28: [GET people/685126/stats](https://statsapi.mlb.com/api/v1/people/685126/stats?stats=gameLog&group=pitching&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:50.642209Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:51.073301Z"}]`. Manifest query ID `d93287baab285934e0567a99e7b0ec724608ca3255b09edd73e260c58761180c`.

<a id="q-7c9bb497c73c7efefbb22e8f5ab5001c9cb08a07c85a317190a53b03fe7fad2c"></a>

FACT: Q-7c9bb497c73c: [GET people/685126/stats](https://statsapi.mlb.com/api/v1/people/685126/stats?stats=gameLog&group=pitching&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:51.149574Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:51.573373Z"}]`. Manifest query ID `7c9bb497c73c7efefbb22e8f5ab5001c9cb08a07c85a317190a53b03fe7fad2c`.

<a id="q-053df08f6d6f41c0a34b2b366926f5407b8e644a7e3812926532481f10cb0bfc"></a>

FACT: Q-053df08f6d6f: [GET people/685126/stats](https://statsapi.mlb.com/api/v1/people/685126/stats?stats=gameLog&group=pitching&season=2023&sportIds=11&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 11, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:51.641103Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:52.073460Z"}]`. Manifest query ID `053df08f6d6f41c0a34b2b366926f5407b8e644a7e3812926532481f10cb0bfc`.

<a id="q-2a5edc5754cc69b843abf09fd23bee6f740b405c2d9b9b34fba7a3de891510bc"></a>

FACT: Q-2a5edc5754cc: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2023&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:52.143653Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:52.573537Z"}]`. Manifest query ID `2a5edc5754cc69b843abf09fd23bee6f740b405c2d9b9b34fba7a3de891510bc`.

<a id="q-f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd"></a>

FACT: Q-f614797863ae: [GET people/695462/stats](https://statsapi.mlb.com/api/v1/people/695462/stats?stats=gameLog&group=hitting&season=2023&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:52.743430Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:53.073629Z"}]`. Manifest query ID `f614797863ae5e40e6573ab4aa8443c9b3df75d38dfa5a1b94c411044f9b35cd`.

<a id="q-a72e5bbaf54220326ec4c28671df425246aa30c6d415da4e732c6e4b6fea39b2"></a>

FACT: Q-a72e5bbaf542: [GET people/695462/stats](https://statsapi.mlb.com/api/v1/people/695462/stats?stats=season&group=hitting&season=2023&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:53.762152Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:53.762186Z"}]`. Manifest query ID `a72e5bbaf54220326ec4c28671df425246aa30c6d415da4e732c6e4b6fea39b2`.

<a id="q-38c40f312ec9d31aac482e26c4a4d11e86409c210e16eed6d136c7a0cbd1eaec"></a>

FACT: Q-38c40f312ec9: [GET people/695462/stats](https://statsapi.mlb.com/api/v1/people/695462/stats?stats=gameLog&group=hitting&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:53.832837Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:54.262269Z"}]`. Manifest query ID `38c40f312ec9d31aac482e26c4a4d11e86409c210e16eed6d136c7a0cbd1eaec`.

<a id="q-5d25cd7a4483fafa0f5b4e29c9c3b718b309aa5be8a33d2541b53e1af715261e"></a>

FACT: Q-5d25cd7a4483: [GET people/695462/stats](https://statsapi.mlb.com/api/v1/people/695462/stats?stats=gameLog&group=hitting&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:54.332078Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:54.762344Z"}]`. Manifest query ID `5d25cd7a4483fafa0f5b4e29c9c3b718b309aa5be8a33d2541b53e1af715261e`.

<a id="q-8fc2b76d01d7e9049fe5e31b4c25888257e74d4439ba46543a3608ead0ed2f89"></a>

FACT: Q-8fc2b76d01d7: [GET people/695462/stats](https://statsapi.mlb.com/api/v1/people/695462/stats?stats=gameLog&group=hitting&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:54.834636Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:55.262415Z"}]`. Manifest query ID `8fc2b76d01d7e9049fe5e31b4c25888257e74d4439ba46543a3608ead0ed2f89`.

<a id="q-4e98d7554ff0bf406e9621991cf423649225dbb14a4f4aea22cc472d20e1c6c2"></a>

FACT: Q-4e98d7554ff0: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2023&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:55.333327Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:55.762491Z"}]`. Manifest query ID `4e98d7554ff0bf406e9621991cf423649225dbb14a4f4aea22cc472d20e1c6c2`.

<a id="q-d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461"></a>

FACT: Q-d8d19b146be8: [GET people/674047/stats](https://statsapi.mlb.com/api/v1/people/674047/stats?stats=gameLog&group=pitching&season=2023&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:55.927307Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:56.262570Z"}]`. Manifest query ID `d8d19b146be8786f6ae6efbe0a013ba449b942e39ade3e469c5597b83569e461`.

<a id="q-da6aa628cbaad9d4e7cdd4700f29059c9a42d5eb918a921676e934a42604583d"></a>

FACT: Q-da6aa628cbaa: [GET people/674047/stats](https://statsapi.mlb.com/api/v1/people/674047/stats?stats=season&group=pitching&season=2023&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:56.347387Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:56.762654Z"}]`. Manifest query ID `da6aa628cbaad9d4e7cdd4700f29059c9a42d5eb918a921676e934a42604583d`.

<a id="q-dce9dc3efb4871a1a7609d736002c8d904329c27a94766239569c88e33b82f92"></a>

FACT: Q-dce9dc3efb48: [GET people/674047/stats](https://statsapi.mlb.com/api/v1/people/674047/stats?stats=gameLog&group=pitching&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:56.846797Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:57.262738Z"}]`. Manifest query ID `dce9dc3efb4871a1a7609d736002c8d904329c27a94766239569c88e33b82f92`.

<a id="q-4c9d35cdc83a5963bf2d21d3889359a174e7868763e79a8f73e6e71eff9bd3ef"></a>

FACT: Q-4c9d35cdc83a: [GET people/674047/stats](https://statsapi.mlb.com/api/v1/people/674047/stats?stats=gameLog&group=pitching&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:57.338380Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:57.762809Z"}]`. Manifest query ID `4c9d35cdc83a5963bf2d21d3889359a174e7868763e79a8f73e6e71eff9bd3ef`.

<a id="q-b0384acb558007bdbe1640120727cb915461f61a26a2b17af0185a26b5313bf3"></a>

FACT: Q-b0384acb5580: [GET people/674047/stats](https://statsapi.mlb.com/api/v1/people/674047/stats?stats=gameLog&group=pitching&season=2023&sportIds=12&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 12, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:57.830853Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:58.262889Z"}]`. Manifest query ID `b0384acb558007bdbe1640120727cb915461f61a26a2b17af0185a26b5313bf3`.

<a id="q-f1f97bfe5707d90baaccb777624b18399efbfc8e66176d7d28fdf66559a85210"></a>

FACT: Q-f1f97bfe5707: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2023&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:58.336452Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:58.763024Z"}]`. Manifest query ID `f1f97bfe5707d90baaccb777624b18399efbfc8e66176d7d28fdf66559a85210`.

<a id="q-d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944"></a>

FACT: Q-d1c462101f53: [GET people/687529/stats](https://statsapi.mlb.com/api/v1/people/687529/stats?stats=gameLog&group=hitting&season=2023&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:58.909835Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:59.263041Z"}]`. Manifest query ID `d1c462101f538efd6350760778c6a820e9509eaad8b606bb864a413d73157944`.

<a id="q-2f7fc438780a902490e861550c768e61c8fcadbfaf8ebac8d551a5d06c8cbecc"></a>

FACT: Q-2f7fc438780a: [GET people/687529/stats](https://statsapi.mlb.com/api/v1/people/687529/stats?stats=season&group=hitting&season=2023&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:59.332848Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:37:59.763114Z"}]`. Manifest query ID `2f7fc438780a902490e861550c768e61c8fcadbfaf8ebac8d551a5d06c8cbecc`.

<a id="q-e09d5ab6bb8e2767cd433e00ac8a5631e2eaf370e1f39bd3afede0ece6fd4c09"></a>

FACT: Q-e09d5ab6bb8e: [GET people/687529/stats](https://statsapi.mlb.com/api/v1/people/687529/stats?stats=gameLog&group=hitting&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:37:59.831936Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:00.263194Z"}]`. Manifest query ID `e09d5ab6bb8e2767cd433e00ac8a5631e2eaf370e1f39bd3afede0ece6fd4c09`.

<a id="q-f7e6f0d85bd65471ac3aa650e94f0029d88b30c9a8afd110d69f91955068b8ee"></a>

FACT: Q-f7e6f0d85bd6: [GET people/687529/stats](https://statsapi.mlb.com/api/v1/people/687529/stats?stats=gameLog&group=hitting&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:00.336214Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:00.763271Z"}]`. Manifest query ID `f7e6f0d85bd65471ac3aa650e94f0029d88b30c9a8afd110d69f91955068b8ee`.

<a id="q-315abcf53b2250051da51e06f208d43a78b47a18b80c05850082ca393252e092"></a>

FACT: Q-315abcf53b22: [GET people/687529/stats](https://statsapi.mlb.com/api/v1/people/687529/stats?stats=gameLog&group=hitting&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:00.834766Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:01.263355Z"}]`. Manifest query ID `315abcf53b2250051da51e06f208d43a78b47a18b80c05850082ca393252e092`.

<a id="q-7e916100cb5bf3eb62952b67f421adeac3485d91a73fe92fedf42eddcfc61e37"></a>

FACT: Q-7e916100cb5b: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2023&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:01.331352Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:01.763429Z"}]`. Manifest query ID `7e916100cb5bf3eb62952b67f421adeac3485d91a73fe92fedf42eddcfc61e37`.

<a id="q-689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31"></a>

FACT: Q-689121cd19ff: [GET people/683409/stats](https://statsapi.mlb.com/api/v1/people/683409/stats?stats=gameLog&group=pitching&season=2023&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:01.920137Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:02.263502Z"}]`. Manifest query ID `689121cd19ff981ae16914d3363b6a0bc020b5354182b0f0bdf9912eb35cfa31`.

<a id="q-6b9f765157c058c19decf2298cb5f23210220ccb6ea0acf521d49c110bf8ceb4"></a>

FACT: Q-6b9f765157c0: [GET people/683409/stats](https://statsapi.mlb.com/api/v1/people/683409/stats?stats=season&group=pitching&season=2023&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:02.338507Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:02.763572Z"}]`. Manifest query ID `6b9f765157c058c19decf2298cb5f23210220ccb6ea0acf521d49c110bf8ceb4`.

<a id="q-dd598791636e0a3b7ff4126680e7852a4dd0b6a10d7f1f46a7bf7d7bce0fedf1"></a>

FACT: Q-dd598791636e: [GET people/683409/stats](https://statsapi.mlb.com/api/v1/people/683409/stats?stats=gameLog&group=pitching&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:02.830612Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:03.263648Z"}]`. Manifest query ID `dd598791636e0a3b7ff4126680e7852a4dd0b6a10d7f1f46a7bf7d7bce0fedf1`.

<a id="q-5682065ec207ef39b139bf47ea943966a42c01c08d75aa4dfad80842bb68c624"></a>

FACT: Q-5682065ec207: [GET people/683409/stats](https://statsapi.mlb.com/api/v1/people/683409/stats?stats=gameLog&group=pitching&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:03.341846Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:03.763720Z"}]`. Manifest query ID `5682065ec207ef39b139bf47ea943966a42c01c08d75aa4dfad80842bb68c624`.

<a id="q-7efb5d29ef8446c316b7dfe697e0406549b1812ff7339cebf5ccb8f34c24b983"></a>

FACT: Q-7efb5d29ef84: [GET people/683409/stats](https://statsapi.mlb.com/api/v1/people/683409/stats?stats=gameLog&group=pitching&season=2023&sportIds=13&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 13, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:03.832254Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:04.263813Z"}]`. Manifest query ID `7efb5d29ef8446c316b7dfe697e0406549b1812ff7339cebf5ccb8f34c24b983`.

<a id="q-b7ed33f3430f6181921b0b6c02eceeffe2df7ad995ac0bf766ea7737e6f4ec32"></a>

FACT: Q-b7ed33f3430f: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2023&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:04.332347Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:04.763888Z"}]`. Manifest query ID `b7ed33f3430f6181921b0b6c02eceeffe2df7ad995ac0bf766ea7737e6f4ec32`.

<a id="q-c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464"></a>

FACT: Q-c799f9cc7a1b: [GET people/689531/stats](https://statsapi.mlb.com/api/v1/people/689531/stats?stats=gameLog&group=hitting&season=2023&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:04.925243Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:05.263981Z"}]`. Manifest query ID `c799f9cc7a1b4304d21f4ef9a77ead51636ed7e401bff3e1de2e720a3e2e7464`.

<a id="q-5efab228cb8cba072b539707500f5c8eab36f6b55212c840972b0051879ec48a"></a>

FACT: Q-5efab228cb8c: [GET people/689531/stats](https://statsapi.mlb.com/api/v1/people/689531/stats?stats=season&group=hitting&season=2023&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2023, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:05.376402Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:05.764040Z"}]`. Manifest query ID `5efab228cb8cba072b539707500f5c8eab36f6b55212c840972b0051879ec48a`.

<a id="q-e66123827da88e0e5b1e83161604c5cf2ec098054fb2f0864325b905f250c028"></a>

FACT: Q-e66123827da8: [GET people/689531/stats](https://statsapi.mlb.com/api/v1/people/689531/stats?stats=gameLog&group=hitting&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:05.843484Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:06.264119Z"}]`. Manifest query ID `e66123827da88e0e5b1e83161604c5cf2ec098054fb2f0864325b905f250c028`.

<a id="q-4345112fe3d5e19e3122bbe481073cd597eff165c7d98ae7cea3367f4eac8960"></a>

FACT: Q-4345112fe3d5: [GET people/689531/stats](https://statsapi.mlb.com/api/v1/people/689531/stats?stats=gameLog&group=hitting&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:06.339715Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:06.764192Z"}]`. Manifest query ID `4345112fe3d5e19e3122bbe481073cd597eff165c7d98ae7cea3367f4eac8960`.

<a id="q-6768de1e1ee12da2ab649b5c58bf58f950aedad9e504959effc85233df68d344"></a>

FACT: Q-6768de1e1ee1: [GET people/689531/stats](https://statsapi.mlb.com/api/v1/people/689531/stats?stats=gameLog&group=hitting&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "hitting", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:06.834770Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:07.264267Z"}]`. Manifest query ID `6768de1e1ee12da2ab649b5c58bf58f950aedad9e504959effc85233df68d344`.

<a id="q-0d3ceee6dfe69e293091c0145d7815ef980e6c911f280121776e503d52e12a35"></a>

FACT: Q-0d3ceee6dfe6: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2023&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2023, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:07.333458Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:07.764342Z"}]`. Manifest query ID `0d3ceee6dfe69e293091c0145d7815ef980e6c911f280121776e503d52e12a35`.

<a id="q-dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc"></a>

FACT: Q-dafaa6625ff3: [GET people/695250/stats](https://statsapi.mlb.com/api/v1/people/695250/stats?stats=gameLog&group=pitching&season=2023&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:07.931225Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:08.264414Z"}]`. Manifest query ID `dafaa6625ff3d4e584ddb7cb859d95229cbec6217a4a7e2ba36a8d3f06956edc`.

<a id="q-df63d9488d33d77a4561cdc9f0619dc234c3c9e4bd00189c31290e12d28c4d7c"></a>

FACT: Q-df63d9488d33: [GET people/695250/stats](https://statsapi.mlb.com/api/v1/people/695250/stats?stats=season&group=pitching&season=2023&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2023, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:08.353826Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:08.764488Z"}]`. Manifest query ID `df63d9488d33d77a4561cdc9f0619dc234c3c9e4bd00189c31290e12d28c4d7c`.

<a id="q-27a9dde0a377d4cf0552c8350d5d5f5da52058ad40783b29b3497f9a39b5b29b"></a>

FACT: Q-27a9dde0a377: [GET people/695250/stats](https://statsapi.mlb.com/api/v1/people/695250/stats?stats=gameLog&group=pitching&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-05-31); parameters=`{"endDate": "2023-05-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:08.837641Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:09.264562Z"}]`. Manifest query ID `27a9dde0a377d4cf0552c8350d5d5f5da52058ad40783b29b3497f9a39b5b29b`.

<a id="q-943dca9adaee4c3a890afec48b23148809404fef248d4b3654eba9b2cd8a0017"></a>

FACT: Q-943dca9adaee: [GET people/695250/stats](https://statsapi.mlb.com/api/v1/people/695250/stats?stats=gameLog&group=pitching&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-07-15); parameters=`{"endDate": "2023-07-15", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:09.339418Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:09.764641Z"}]`. Manifest query ID `943dca9adaee4c3a890afec48b23148809404fef248d4b3654eba9b2cd8a0017`.

<a id="q-25af71cd6fffc0163f7b7ed34a1d2652fc48c33d5d2e585723436c60f2e2c5d2"></a>

FACT: Q-25af71cd6fff: [GET people/695250/stats](https://statsapi.mlb.com/api/v1/people/695250/stats?stats=gameLog&group=pitching&season=2023&sportIds=14&gameType=R&startDate=2023-01-01&endDate=2023-08-31); parameters=`{"endDate": "2023-08-31", "gameType": "R", "group": "pitching", "season": 2023, "sportIds": 14, "startDate": "2023-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:09.833442Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:10.264717Z"}]`. Manifest query ID `25af71cd6fffc0163f7b7ed34a1d2652fc48c33d5d2e585723436c60f2e2c5d2`.

<a id="q-0be3b9bfced55d7fea1fe05011de6be56cd5d93fa515d0af2fdbe6bfa16da8e2"></a>

FACT: Q-0be3b9bfced5: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2024&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:10.335433Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:10.764796Z"}]`. Manifest query ID `0be3b9bfced55d7fea1fe05011de6be56cd5d93fa515d0af2fdbe6bfa16da8e2`.

<a id="q-774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6"></a>

FACT: Q-774a1837da10: [GET people/682877/stats](https://statsapi.mlb.com/api/v1/people/682877/stats?stats=gameLog&group=hitting&season=2024&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:10.889418Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:11.264870Z"}]`. Manifest query ID `774a1837da108d5901e5758e86c20b0fb79da6a2d6cd8acb7a2e66e9151c1ad6`.

<a id="q-1477828214dc47eb235374c9480c7f1d882cb74a4b8dbefec0b783e137d7f7d2"></a>

FACT: Q-1477828214dc: [GET people/682877/stats](https://statsapi.mlb.com/api/v1/people/682877/stats?stats=season&group=hitting&season=2024&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:11.341822Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:11.764943Z"}]`. Manifest query ID `1477828214dc47eb235374c9480c7f1d882cb74a4b8dbefec0b783e137d7f7d2`.

<a id="q-0d19a54c518df917235e7ee898d8bae09a8ddc7080fe6a989379adebef2bb5a5"></a>

FACT: Q-0d19a54c518d: [GET people/682877/stats](https://statsapi.mlb.com/api/v1/people/682877/stats?stats=gameLog&group=hitting&season=2024&sportIds=11&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 11, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:11.839274Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:12.265042Z"}]`. Manifest query ID `0d19a54c518df917235e7ee898d8bae09a8ddc7080fe6a989379adebef2bb5a5`.

<a id="q-cbb07acf50ee67bef36434ffb411660a217b010ae15fa7735140c7f32342035f"></a>

FACT: Q-cbb07acf50ee: [GET people/682877/stats](https://statsapi.mlb.com/api/v1/people/682877/stats?stats=gameLog&group=hitting&season=2024&sportIds=11&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 11, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:12.335327Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:12.765115Z"}]`. Manifest query ID `cbb07acf50ee67bef36434ffb411660a217b010ae15fa7735140c7f32342035f`.

<a id="q-e3457da070e4e4b32b1c48a7c9605c6c29649a31f867a8fd16ab25ac654fd1a6"></a>

FACT: Q-e3457da070e4: [GET people/682877/stats](https://statsapi.mlb.com/api/v1/people/682877/stats?stats=gameLog&group=hitting&season=2024&sportIds=11&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 11, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:12.835344Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:13.265168Z"}]`. Manifest query ID `e3457da070e4e4b32b1c48a7c9605c6c29649a31f867a8fd16ab25ac654fd1a6`.

<a id="q-3bfe13436817d5295d57348650d13b537718eebe778070484ea92d51a54102ae"></a>

FACT: Q-3bfe13436817: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2024&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:13.334091Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:13.765249Z"}]`. Manifest query ID `3bfe13436817d5295d57348650d13b537718eebe778070484ea92d51a54102ae`.

<a id="q-03826c32ecc97cd757f4aeb680c61e9d00913c748f598d9888974c2b9f9170d2"></a>

FACT: Q-03826c32ecc9: [GET people/593833/stats](https://statsapi.mlb.com/api/v1/people/593833/stats?stats=gameLog&group=pitching&season=2024&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:13.989337Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:14.265322Z"}]`. Manifest query ID `03826c32ecc97cd757f4aeb680c61e9d00913c748f598d9888974c2b9f9170d2`.

<a id="q-1509354dc23ec61e338d9a3ddffa560f8087b3b2ecb4011c9f1dafc1315eff35"></a>

FACT: Q-1509354dc23e: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2024&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:14.343914Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:14.765396Z"}]`. Manifest query ID `1509354dc23ec61e338d9a3ddffa560f8087b3b2ecb4011c9f1dafc1315eff35`.

<a id="q-cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba"></a>

FACT: Q-cc29b39f070f: [GET people/691442/stats](https://statsapi.mlb.com/api/v1/people/691442/stats?stats=gameLog&group=hitting&season=2024&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:14.895507Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:15.265476Z"}]`. Manifest query ID `cc29b39f070fb9486dbacd51e7cee761f43dea8a17a8b6461a2eaedf31a5f8ba`.

<a id="q-c391660b5b51c6376a92b132f7a66b21465a2c453c32d57cb0bdfef8eb832167"></a>

FACT: Q-c391660b5b51: [GET people/691442/stats](https://statsapi.mlb.com/api/v1/people/691442/stats?stats=season&group=hitting&season=2024&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:15.769497Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:15.769553Z"}]`. Manifest query ID `c391660b5b51c6376a92b132f7a66b21465a2c453c32d57cb0bdfef8eb832167`.

<a id="q-67f87ffbeb949e5da966bf9a1d056a8f0a9fe10e7d040aafebbd3d358b4108d0"></a>

FACT: Q-67f87ffbeb94: [GET people/691442/stats](https://statsapi.mlb.com/api/v1/people/691442/stats?stats=gameLog&group=hitting&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:15.840362Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:16.269630Z"}]`. Manifest query ID `67f87ffbeb949e5da966bf9a1d056a8f0a9fe10e7d040aafebbd3d358b4108d0`.

<a id="q-c4798ad79c5519bde13a754da8273a285cd0c677912e0fa97a6e0d0397b4f0bd"></a>

FACT: Q-c4798ad79c55: [GET people/691442/stats](https://statsapi.mlb.com/api/v1/people/691442/stats?stats=gameLog&group=hitting&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:16.359411Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:16.769723Z"}]`. Manifest query ID `c4798ad79c5519bde13a754da8273a285cd0c677912e0fa97a6e0d0397b4f0bd`.

<a id="q-9c7368c811d9b0719469870b80d8de68b6dd9d0f7508db3da2819d067e240d9a"></a>

FACT: Q-9c7368c811d9: [GET people/691442/stats](https://statsapi.mlb.com/api/v1/people/691442/stats?stats=gameLog&group=hitting&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:16.841051Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:17.269812Z"}]`. Manifest query ID `9c7368c811d9b0719469870b80d8de68b6dd9d0f7508db3da2819d067e240d9a`.

<a id="q-b27982099b8b3b969be7c08a9748c13c4cb22a8b34765f03541d15e9c50a3f3c"></a>

FACT: Q-b27982099b8b: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2024&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:17.346969Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:17.769886Z"}]`. Manifest query ID `b27982099b8b3b969be7c08a9748c13c4cb22a8b34765f03541d15e9c50a3f3c`.

<a id="q-35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f"></a>

FACT: Q-35db42c0941d: [GET people/694335/stats](https://statsapi.mlb.com/api/v1/people/694335/stats?stats=gameLog&group=pitching&season=2024&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:17.926061Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:18.269960Z"}]`. Manifest query ID `35db42c0941dc8a241af7839d642b4253b574d8e28b74a3cdba8b0f95d8dd74f`.

<a id="q-ed4416c37c30f4461779cab1d3953fef4047a8b2c6e680b78b18faa0e7fd1689"></a>

FACT: Q-ed4416c37c30: [GET people/694335/stats](https://statsapi.mlb.com/api/v1/people/694335/stats?stats=season&group=pitching&season=2024&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:18.350955Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:18.770032Z"}]`. Manifest query ID `ed4416c37c30f4461779cab1d3953fef4047a8b2c6e680b78b18faa0e7fd1689`.

<a id="q-3cab11cc507793f65d4f321fc3a9996e43969171750e8a1ee200dff482f4252d"></a>

FACT: Q-3cab11cc5077: [GET people/694335/stats](https://statsapi.mlb.com/api/v1/people/694335/stats?stats=gameLog&group=pitching&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:18.839975Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:19.270105Z"}]`. Manifest query ID `3cab11cc507793f65d4f321fc3a9996e43969171750e8a1ee200dff482f4252d`.

<a id="q-13f3f469fa8cdd01855034ccd79ad3c3e041426eb8afd8d01312c01fa4601932"></a>

FACT: Q-13f3f469fa8c: [GET people/694335/stats](https://statsapi.mlb.com/api/v1/people/694335/stats?stats=gameLog&group=pitching&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:19.339940Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:19.770181Z"}]`. Manifest query ID `13f3f469fa8cdd01855034ccd79ad3c3e041426eb8afd8d01312c01fa4601932`.

<a id="q-dbfdea5b61a7b94b057649b15ab44af83030648c47219aa3138f7e80d09c9b95"></a>

FACT: Q-dbfdea5b61a7: [GET people/694335/stats](https://statsapi.mlb.com/api/v1/people/694335/stats?stats=gameLog&group=pitching&season=2024&sportIds=12&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 12, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:19.840470Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:20.270324Z"}]`. Manifest query ID `dbfdea5b61a7b94b057649b15ab44af83030648c47219aa3138f7e80d09c9b95`.

<a id="q-792bd089985d7f77cd075bdddfa939d5e4c87e3c18234b124c78448c16175151"></a>

FACT: Q-792bd089985d: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2024&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:20.341462Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:20.770399Z"}]`. Manifest query ID `792bd089985d7f77cd075bdddfa939d5e4c87e3c18234b124c78448c16175151`.

<a id="q-eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502"></a>

FACT: Q-eca4a6ee9f5a: [GET people/695521/stats](https://statsapi.mlb.com/api/v1/people/695521/stats?stats=gameLog&group=hitting&season=2024&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:20.909035Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:21.270491Z"}]`. Manifest query ID `eca4a6ee9f5aeb50714ac941929b243de9009583763e78f286d528c444cd6502`.

<a id="q-db697d69c7072ba17cdce3d3de49dcef1c32985e435f80a41621f4941fe38f2c"></a>

FACT: Q-db697d69c707: [GET people/695521/stats](https://statsapi.mlb.com/api/v1/people/695521/stats?stats=season&group=hitting&season=2024&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:21.767713Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:21.770550Z"}]`. Manifest query ID `db697d69c7072ba17cdce3d3de49dcef1c32985e435f80a41621f4941fe38f2c`.

<a id="q-a85cdfd6a265d8e93e8454e0bed51f5c32f461693cbb4801c3194673fd6f2bfb"></a>

FACT: Q-a85cdfd6a265: [GET people/695521/stats](https://statsapi.mlb.com/api/v1/people/695521/stats?stats=gameLog&group=hitting&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:21.840020Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:22.270630Z"}]`. Manifest query ID `a85cdfd6a265d8e93e8454e0bed51f5c32f461693cbb4801c3194673fd6f2bfb`.

<a id="q-c3b5c7d608c793f52660ff0001d54bef0531ae9d8250b9aed9712dd86f6e348b"></a>

FACT: Q-c3b5c7d608c7: [GET people/695521/stats](https://statsapi.mlb.com/api/v1/people/695521/stats?stats=gameLog&group=hitting&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:22.349939Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:22.770733Z"}]`. Manifest query ID `c3b5c7d608c793f52660ff0001d54bef0531ae9d8250b9aed9712dd86f6e348b`.

<a id="q-a650ecaaf35af484cbd32440b46d0b17d45399e3ff015c7d299758d891d98e9f"></a>

FACT: Q-a650ecaaf35a: [GET people/695521/stats](https://statsapi.mlb.com/api/v1/people/695521/stats?stats=gameLog&group=hitting&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:22.845271Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:23.270819Z"}]`. Manifest query ID `a650ecaaf35af484cbd32440b46d0b17d45399e3ff015c7d299758d891d98e9f`.

<a id="q-11cd6b541afc062f842bd14ff543bc08e98f1c784fddb8d88323a1f8be63341a"></a>

FACT: Q-11cd6b541afc: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2024&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:23.341121Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:23.770892Z"}]`. Manifest query ID `11cd6b541afc062f842bd14ff543bc08e98f1c784fddb8d88323a1f8be63341a`.

<a id="q-ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798"></a>

FACT: Q-ddb875a08dd6: [GET people/806362/stats](https://statsapi.mlb.com/api/v1/people/806362/stats?stats=gameLog&group=pitching&season=2024&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:23.941764Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:24.270970Z"}]`. Manifest query ID `ddb875a08dd660db9a2eb2aa2a216b1dbae012f370c9210b9a77ec874120b798`.

<a id="q-1d92915686a1dc9de6b60fff25819c475678a160ab3f8b4e741d4bfd454a615c"></a>

FACT: Q-1d92915686a1: [GET people/806362/stats](https://statsapi.mlb.com/api/v1/people/806362/stats?stats=season&group=pitching&season=2024&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:24.339739Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:24.771035Z"}]`. Manifest query ID `1d92915686a1dc9de6b60fff25819c475678a160ab3f8b4e741d4bfd454a615c`.

<a id="q-c202bc5df9140d5d3561d21b16b16bcd7e7881b17eb691c404e2f7f5025e9160"></a>

FACT: Q-c202bc5df914: [GET people/806362/stats](https://statsapi.mlb.com/api/v1/people/806362/stats?stats=gameLog&group=pitching&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:24.845901Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:25.271110Z"}]`. Manifest query ID `c202bc5df9140d5d3561d21b16b16bcd7e7881b17eb691c404e2f7f5025e9160`.

<a id="q-b22a61a18c1c0782371ab9dc8830fc6d23c5aa71ecb8f002b62f2e1bec66edbe"></a>

FACT: Q-b22a61a18c1c: [GET people/806362/stats](https://statsapi.mlb.com/api/v1/people/806362/stats?stats=gameLog&group=pitching&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:25.343499Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:25.771182Z"}]`. Manifest query ID `b22a61a18c1c0782371ab9dc8830fc6d23c5aa71ecb8f002b62f2e1bec66edbe`.

<a id="q-ec39f58bde55408975d75d6f506b4dddc69c3e93424830b90008ef5f30e20e28"></a>

FACT: Q-ec39f58bde55: [GET people/806362/stats](https://statsapi.mlb.com/api/v1/people/806362/stats?stats=gameLog&group=pitching&season=2024&sportIds=13&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 13, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:25.845073Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:26.271270Z"}]`. Manifest query ID `ec39f58bde55408975d75d6f506b4dddc69c3e93424830b90008ef5f30e20e28`.

<a id="q-c9466fcde4175df4878eba8272d607842c90606d16b39dd56ce4669fc46bbce1"></a>

FACT: Q-c9466fcde417: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2024&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:26.346568Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:26.771346Z"}]`. Manifest query ID `c9466fcde4175df4878eba8272d607842c90606d16b39dd56ce4669fc46bbce1`.

<a id="q-47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982"></a>

FACT: Q-47fcf69fecc4: [GET people/703149/stats](https://statsapi.mlb.com/api/v1/people/703149/stats?stats=gameLog&group=hitting&season=2024&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:26.905044Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:27.271442Z"}]`. Manifest query ID `47fcf69fecc407fc53c1ba70520f1087b64391da9b9234d9308ebebd078f8982`.

<a id="q-7fb8eecdc4716eb386a50e1436bce29211d088173e87152e51ba081762c248c8"></a>

FACT: Q-7fb8eecdc471: [GET people/703149/stats](https://statsapi.mlb.com/api/v1/people/703149/stats?stats=season&group=hitting&season=2024&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2024, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:27.772863Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:27.772890Z"}]`. Manifest query ID `7fb8eecdc4716eb386a50e1436bce29211d088173e87152e51ba081762c248c8`.

<a id="q-ed0560c652874b76a7d0458fab0fae4bdc2a7ecb8843ed6c241d0c031ecb111a"></a>

FACT: Q-ed0560c65287: [GET people/703149/stats](https://statsapi.mlb.com/api/v1/people/703149/stats?stats=gameLog&group=hitting&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:27.839363Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:28.272977Z"}]`. Manifest query ID `ed0560c652874b76a7d0458fab0fae4bdc2a7ecb8843ed6c241d0c031ecb111a`.

<a id="q-778f9922901bce4a3b62b105aad7eb56f67effa1c8f3bac6aafa5ff58a98f5b9"></a>

FACT: Q-778f9922901b: [GET people/703149/stats](https://statsapi.mlb.com/api/v1/people/703149/stats?stats=gameLog&group=hitting&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:28.348020Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:28.773055Z"}]`. Manifest query ID `778f9922901bce4a3b62b105aad7eb56f67effa1c8f3bac6aafa5ff58a98f5b9`.

<a id="q-dccab47b0d6c65e08fff1df6e22355106c2b55f1ab1817220d4ccaf91a4bdb97"></a>

FACT: Q-dccab47b0d6c: [GET people/703149/stats](https://statsapi.mlb.com/api/v1/people/703149/stats?stats=gameLog&group=hitting&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "hitting", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:28.843513Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:29.273131Z"}]`. Manifest query ID `dccab47b0d6c65e08fff1df6e22355106c2b55f1ab1817220d4ccaf91a4bdb97`.

<a id="q-191ba00c8bf2cc6603356720aa0636306b622de11b1d265c0f82f01d67fb7e59"></a>

FACT: Q-191ba00c8bf2: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2024&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2024, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:29.348163Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:29.773201Z"}]`. Manifest query ID `191ba00c8bf2cc6603356720aa0636306b622de11b1d265c0f82f01d67fb7e59`.

<a id="q-3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956"></a>

FACT: Q-3aac98bc3ad8: [GET people/692624/stats](https://statsapi.mlb.com/api/v1/people/692624/stats?stats=gameLog&group=pitching&season=2024&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:29.981515Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:30.273280Z"}]`. Manifest query ID `3aac98bc3ad812a2143abdf007c965f801d6f60fbf0977a6ef28dc8dc8fe8956`.

<a id="q-39d1de4b2038eae42e719a53bece450b8a2cc01b9860b9816eea832352d77917"></a>

FACT: Q-39d1de4b2038: [GET people/692624/stats](https://statsapi.mlb.com/api/v1/people/692624/stats?stats=season&group=pitching&season=2024&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2024, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:30.345155Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:30.773358Z"}]`. Manifest query ID `39d1de4b2038eae42e719a53bece450b8a2cc01b9860b9816eea832352d77917`.

<a id="q-dfdd7cafc10cb292773dda2df2d5e51af6b0669aa9289e2cc706ecdbc0adb931"></a>

FACT: Q-dfdd7cafc10c: [GET people/692624/stats](https://statsapi.mlb.com/api/v1/people/692624/stats?stats=gameLog&group=pitching&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-05-31); parameters=`{"endDate": "2024-05-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:30.852503Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:31.273460Z"}]`. Manifest query ID `dfdd7cafc10cb292773dda2df2d5e51af6b0669aa9289e2cc706ecdbc0adb931`.

<a id="q-e1967b9b6bf84f823be3bc0729d658ee94630d284c6d8211009cd0f22fe2ce92"></a>

FACT: Q-e1967b9b6bf8: [GET people/692624/stats](https://statsapi.mlb.com/api/v1/people/692624/stats?stats=gameLog&group=pitching&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-07-15); parameters=`{"endDate": "2024-07-15", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:31.347734Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:31.773535Z"}]`. Manifest query ID `e1967b9b6bf84f823be3bc0729d658ee94630d284c6d8211009cd0f22fe2ce92`.

<a id="q-4f13475daaeaa1268c11da751c88896c5587cdd6f880194ee7b75ee8eddf3b12"></a>

FACT: Q-4f13475daaea: [GET people/692624/stats](https://statsapi.mlb.com/api/v1/people/692624/stats?stats=gameLog&group=pitching&season=2024&sportIds=14&gameType=R&startDate=2024-01-01&endDate=2024-08-31); parameters=`{"endDate": "2024-08-31", "gameType": "R", "group": "pitching", "season": 2024, "sportIds": 14, "startDate": "2024-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:31.857438Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:32.273625Z"}]`. Manifest query ID `4f13475daaeaa1268c11da751c88896c5587cdd6f880194ee7b75ee8eddf3b12`.

<a id="q-8e956eb23007daa3bbf0531dc788127d52495ec4a262b1b0708e8cd31e6269b3"></a>

FACT: Q-8e956eb23007: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2025&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:32.343370Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:32.773695Z"}]`. Manifest query ID `8e956eb23007daa3bbf0531dc788127d52495ec4a262b1b0708e8cd31e6269b3`.

<a id="q-31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456"></a>

FACT: Q-31e3d6b298b9: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2025&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:32.911191Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:33.273781Z"}]`. Manifest query ID `31e3d6b298b95730081027c7fb3e608bc8bb67049421e7daf8974fe40494b456`.

<a id="q-108fc6908d14529ce4a1da59459154ba8503453f0bd1b3472713ca580f33a300"></a>

FACT: Q-108fc6908d14: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=season&group=hitting&season=2025&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:33.345802Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:33.773871Z"}]`. Manifest query ID `108fc6908d14529ce4a1da59459154ba8503453f0bd1b3472713ca580f33a300`.

<a id="q-861a6d82009721f158ca10785f1f514a3afe3f8ca58942f042e7792a53248afa"></a>

FACT: Q-861a6d820097: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:33.846017Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:34.273950Z"}]`. Manifest query ID `861a6d82009721f158ca10785f1f514a3afe3f8ca58942f042e7792a53248afa`.

<a id="q-544b0dd605da8635778e20d03f19c209818a37d08a79721a757b79eb273c13f6"></a>

FACT: Q-544b0dd605da: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:34.350786Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:34.774037Z"}]`. Manifest query ID `544b0dd605da8635778e20d03f19c209818a37d08a79721a757b79eb273c13f6`.

<a id="q-e03f080abd6ec940784bce55132571440ce0e27f65cde2bc427c64ce2c13d464"></a>

FACT: Q-e03f080abd6e: [GET people/669899/stats](https://statsapi.mlb.com/api/v1/people/669899/stats?stats=gameLog&group=hitting&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:34.847788Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:35.274119Z"}]`. Manifest query ID `e03f080abd6ec940784bce55132571440ce0e27f65cde2bc427c64ce2c13d464`.

<a id="q-351e85ce3426b3ca936091f49a447bd6413cc850e941116b5dac256c42cc4fbd"></a>

FACT: Q-351e85ce3426: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2025&sportIds=11&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:35.345543Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:35.774206Z"}]`. Manifest query ID `351e85ce3426b3ca936091f49a447bd6413cc850e941116b5dac256c42cc4fbd`.

<a id="q-3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b"></a>

FACT: Q-3e7236e6253b: [GET people/665996/stats](https://statsapi.mlb.com/api/v1/people/665996/stats?stats=gameLog&group=pitching&season=2025&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 11, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:35.939703Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:36.274295Z"}]`. Manifest query ID `3e7236e6253b0b22e64a86574c14d3bd9f623ebd87de0a4ab4ae859ac044b48b`.

<a id="q-e7461681446f21d2dc51b217d6a2b9d5492baf6f9c6fd67bfddb588c426f9b8e"></a>

FACT: Q-e7461681446f: [GET people/665996/stats](https://statsapi.mlb.com/api/v1/people/665996/stats?stats=season&group=pitching&season=2025&sportIds=11&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 11, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:36.360360Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:36.774377Z"}]`. Manifest query ID `e7461681446f21d2dc51b217d6a2b9d5492baf6f9c6fd67bfddb588c426f9b8e`.

<a id="q-6ee154426edd4384b72c3026c308390edbd4a8e70bb0278bb11fdf9b54996de6"></a>

FACT: Q-6ee154426edd: [GET people/665996/stats](https://statsapi.mlb.com/api/v1/people/665996/stats?stats=gameLog&group=pitching&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:36.859141Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:37.274465Z"}]`. Manifest query ID `6ee154426edd4384b72c3026c308390edbd4a8e70bb0278bb11fdf9b54996de6`.

<a id="q-7acd8a03b6030e73c43fe355c526f595656f422926c7c7b8cd8bffde2b55ad22"></a>

FACT: Q-7acd8a03b603: [GET people/665996/stats](https://statsapi.mlb.com/api/v1/people/665996/stats?stats=gameLog&group=pitching&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:37.366329Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:37.774562Z"}]`. Manifest query ID `7acd8a03b6030e73c43fe355c526f595656f422926c7c7b8cd8bffde2b55ad22`.

<a id="q-437aa523ef7348fa3c068cc71a3c0489a767c808a43dceff7671a28daa05070a"></a>

FACT: Q-437aa523ef73: [GET people/665996/stats](https://statsapi.mlb.com/api/v1/people/665996/stats?stats=gameLog&group=pitching&season=2025&sportIds=11&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 11, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:37.870177Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:38.274637Z"}]`. Manifest query ID `437aa523ef7348fa3c068cc71a3c0489a767c808a43dceff7671a28daa05070a`.

<a id="q-9aecef586a685812354a731bacd817e16d85a755b83de0c6b92918d1dd6ef37e"></a>

FACT: Q-9aecef586a68: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2025&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:38.368185Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:38.774726Z"}]`. Manifest query ID `9aecef586a685812354a731bacd817e16d85a755b83de0c6b92918d1dd6ef37e`.

<a id="q-5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6"></a>

FACT: Q-5e58cdf8040d: [GET people/800325/stats](https://statsapi.mlb.com/api/v1/people/800325/stats?stats=gameLog&group=hitting&season=2025&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:38.924841Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:39.274806Z"}]`. Manifest query ID `5e58cdf8040dca2cda30ca31bdb7d9a344e885524775b6b38386c2ff317fb2c6`.

<a id="q-2833e489161d2e59024bcd33d0cce56cb0fd05266849eb3cced26995b81563ca"></a>

FACT: Q-2833e489161d: [GET people/800325/stats](https://statsapi.mlb.com/api/v1/people/800325/stats?stats=season&group=hitting&season=2025&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:39.346061Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:39.774879Z"}]`. Manifest query ID `2833e489161d2e59024bcd33d0cce56cb0fd05266849eb3cced26995b81563ca`.

<a id="q-c370e0b57b3e885e065ea7a5e8552a6af27490d2ad885c2bc13876437189f836"></a>

FACT: Q-c370e0b57b3e: [GET people/800325/stats](https://statsapi.mlb.com/api/v1/people/800325/stats?stats=gameLog&group=hitting&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:39.846970Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:40.274955Z"}]`. Manifest query ID `c370e0b57b3e885e065ea7a5e8552a6af27490d2ad885c2bc13876437189f836`.

<a id="q-8d7a464ece7c0f7f894437d8af4b1c8e78ba7c9d51118a62e6ab9ea786e800e0"></a>

FACT: Q-8d7a464ece7c: [GET people/800325/stats](https://statsapi.mlb.com/api/v1/people/800325/stats?stats=gameLog&group=hitting&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:40.348540Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:40.775050Z"}]`. Manifest query ID `8d7a464ece7c0f7f894437d8af4b1c8e78ba7c9d51118a62e6ab9ea786e800e0`.

<a id="q-6f36fe335696007de991c0a712a3a381f2a3cbe8bfa8fd780f0ea2261e02c26c"></a>

FACT: Q-6f36fe335696: [GET people/800325/stats](https://statsapi.mlb.com/api/v1/people/800325/stats?stats=gameLog&group=hitting&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:40.852861Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:41.275137Z"}]`. Manifest query ID `6f36fe335696007de991c0a712a3a381f2a3cbe8bfa8fd780f0ea2261e02c26c`.

<a id="q-27ab2d8b4c421ada4df4cdc01f00a4250f43d210bd9ca3bb9c7dc1fa5f008834"></a>

FACT: Q-27ab2d8b4c42: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2025&sportIds=12&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:41.359745Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:41.775227Z"}]`. Manifest query ID `27ab2d8b4c421ada4df4cdc01f00a4250f43d210bd9ca3bb9c7dc1fa5f008834`.

<a id="q-ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5"></a>

FACT: Q-ac2d070af302: [GET people/681744/stats](https://statsapi.mlb.com/api/v1/people/681744/stats?stats=gameLog&group=pitching&season=2025&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 12, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:41.955293Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:42.275316Z"}]`. Manifest query ID `ac2d070af3025426ff527842883ba74b721d333ed3c0651800105c569ebfb0d5`.

<a id="q-603ed2b79ba8fa883375ee775564fff56295019fde19f03df4ace921d6395b9e"></a>

FACT: Q-603ed2b79ba8: [GET people/681744/stats](https://statsapi.mlb.com/api/v1/people/681744/stats?stats=season&group=pitching&season=2025&sportIds=12&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 12, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:42.346170Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:42.775391Z"}]`. Manifest query ID `603ed2b79ba8fa883375ee775564fff56295019fde19f03df4ace921d6395b9e`.

<a id="q-0492691e573da6a4178e085bd8715285541ad4a2ce5065a0fbb0ea862de4560f"></a>

FACT: Q-0492691e573d: [GET people/681744/stats](https://statsapi.mlb.com/api/v1/people/681744/stats?stats=gameLog&group=pitching&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:42.845018Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:43.275469Z"}]`. Manifest query ID `0492691e573da6a4178e085bd8715285541ad4a2ce5065a0fbb0ea862de4560f`.

<a id="q-89c46961b3ff6e2f5734f40a8a0fa9ba9b17499dd6a98962e19e82bb7e02903e"></a>

FACT: Q-89c46961b3ff: [GET people/681744/stats](https://statsapi.mlb.com/api/v1/people/681744/stats?stats=gameLog&group=pitching&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:43.343824Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:43.775549Z"}]`. Manifest query ID `89c46961b3ff6e2f5734f40a8a0fa9ba9b17499dd6a98962e19e82bb7e02903e`.

<a id="q-ca881ab45163475157d599ffe1cda07b3e80551dd93ca0adc6a87b73b9c0a7ea"></a>

FACT: Q-ca881ab45163: [GET people/681744/stats](https://statsapi.mlb.com/api/v1/people/681744/stats?stats=gameLog&group=pitching&season=2025&sportIds=12&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 12, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:43.848469Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:44.275657Z"}]`. Manifest query ID `ca881ab45163475157d599ffe1cda07b3e80551dd93ca0adc6a87b73b9c0a7ea`.

<a id="q-d19ea932a5e849689f18ce7c14ff03da8946be4bd8c1fce7ffc1b755b50df0ea"></a>

FACT: Q-d19ea932a5e8: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2025&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:44.349211Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:44.775728Z"}]`. Manifest query ID `d19ea932a5e849689f18ce7c14ff03da8946be4bd8c1fce7ffc1b755b50df0ea`.

<a id="q-aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236"></a>

FACT: Q-aaaac569e68a: [GET people/813841/stats](https://statsapi.mlb.com/api/v1/people/813841/stats?stats=gameLog&group=hitting&season=2025&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:44.900233Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:45.275815Z"}]`. Manifest query ID `aaaac569e68a2fa5554de2270160a8e4365448c22d4a9b471eeb213c1f104236`.

<a id="q-06994f76c3d9a8acaa930f1bdc4dd63000805fa7abd77158aea82b9ec44a2f7a"></a>

FACT: Q-06994f76c3d9: [GET people/813841/stats](https://statsapi.mlb.com/api/v1/people/813841/stats?stats=season&group=hitting&season=2025&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:45.359094Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:45.775889Z"}]`. Manifest query ID `06994f76c3d9a8acaa930f1bdc4dd63000805fa7abd77158aea82b9ec44a2f7a`.

<a id="q-f55dece0bde881ef2d2c4c2621aefd2ff38801bc79fe522976c9a58111d99eb6"></a>

FACT: Q-f55dece0bde8: [GET people/813841/stats](https://statsapi.mlb.com/api/v1/people/813841/stats?stats=gameLog&group=hitting&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:45.854802Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:46.275972Z"}]`. Manifest query ID `f55dece0bde881ef2d2c4c2621aefd2ff38801bc79fe522976c9a58111d99eb6`.

<a id="q-cbbfba42feab6dc155e659257777828783dcc59ec21bd87a25322407bff6040b"></a>

FACT: Q-cbbfba42feab: [GET people/813841/stats](https://statsapi.mlb.com/api/v1/people/813841/stats?stats=gameLog&group=hitting&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:46.349751Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:46.776052Z"}]`. Manifest query ID `cbbfba42feab6dc155e659257777828783dcc59ec21bd87a25322407bff6040b`.

<a id="q-cb3762f69b59af74fa08eb0e32b2be0a1a89a7fa4e6d2a59c9cb4cdb3b34a3c1"></a>

FACT: Q-cb3762f69b59: [GET people/813841/stats](https://statsapi.mlb.com/api/v1/people/813841/stats?stats=gameLog&group=hitting&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:46.846207Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:47.276125Z"}]`. Manifest query ID `cb3762f69b59af74fa08eb0e32b2be0a1a89a7fa4e6d2a59c9cb4cdb3b34a3c1`.

<a id="q-99dd5fa81ab203fa5d5b35daf5a03743534d2bf8b507b8bcbdc69e0ab7f6c44f"></a>

FACT: Q-99dd5fa81ab2: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2025&sportIds=13&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:47.373724Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:47.776230Z"}]`. Manifest query ID `99dd5fa81ab203fa5d5b35daf5a03743534d2bf8b507b8bcbdc69e0ab7f6c44f`.

<a id="q-cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc"></a>

FACT: Q-cfa54bef48c6: [GET people/681060/stats](https://statsapi.mlb.com/api/v1/people/681060/stats?stats=gameLog&group=pitching&season=2025&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 13, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:47.939964Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:48.276314Z"}]`. Manifest query ID `cfa54bef48c6ba2d1c2c66906406df8aee381cc6aaa84f428af32b00075f90dc`.

<a id="q-5825a4a809f239e719363f8bc0f629b05588bc6881750daf88cdbd6d4688aac3"></a>

FACT: Q-5825a4a809f2: [GET people/681060/stats](https://statsapi.mlb.com/api/v1/people/681060/stats?stats=season&group=pitching&season=2025&sportIds=13&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 13, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:48.352200Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:48.776401Z"}]`. Manifest query ID `5825a4a809f239e719363f8bc0f629b05588bc6881750daf88cdbd6d4688aac3`.

<a id="q-1ebcee9b128bdae645669f279caac0b281415e4d0f8073d3fbde4ba893400b51"></a>

FACT: Q-1ebcee9b128b: [GET people/681060/stats](https://statsapi.mlb.com/api/v1/people/681060/stats?stats=gameLog&group=pitching&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:48.850800Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:49.276485Z"}]`. Manifest query ID `1ebcee9b128bdae645669f279caac0b281415e4d0f8073d3fbde4ba893400b51`.

<a id="q-b9fbd9f2b04d32ab47b0668696423f0a2586743c41839c8ba5da157dc9b629b0"></a>

FACT: Q-b9fbd9f2b04d: [GET people/681060/stats](https://statsapi.mlb.com/api/v1/people/681060/stats?stats=gameLog&group=pitching&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:49.353629Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:49.776561Z"}]`. Manifest query ID `b9fbd9f2b04d32ab47b0668696423f0a2586743c41839c8ba5da157dc9b629b0`.

<a id="q-df151c6aab5be1241bdfd49e3719d9c7745ac8c85b1ed270d9bf4dc3bd62f5c0"></a>

FACT: Q-df151c6aab5b: [GET people/681060/stats](https://statsapi.mlb.com/api/v1/people/681060/stats?stats=gameLog&group=pitching&season=2025&sportIds=13&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 13, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:49.848584Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:50.276634Z"}]`. Manifest query ID `df151c6aab5be1241bdfd49e3719d9c7745ac8c85b1ed270d9bf4dc3bd62f5c0`.

<a id="q-1f10fbfb1d281563010ff4174c8e7c684565c6c03f938bd7fe043b63b6effc1e"></a>

FACT: Q-1f10fbfb1d28: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=hitting&season=2025&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "hitting", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:50.347569Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:50.776707Z"}]`. Manifest query ID `1f10fbfb1d281563010ff4174c8e7c684565c6c03f938bd7fe043b63b6effc1e`.

<a id="q-8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62"></a>

FACT: Q-8aaea0b14b9e: [GET people/804558/stats](https://statsapi.mlb.com/api/v1/people/804558/stats?stats=gameLog&group=hitting&season=2025&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:50.917196Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:51.276784Z"}]`. Manifest query ID `8aaea0b14b9e1568f301f27566c7ce5aba475724f9befae1d5b3c43b660fcf62`.

<a id="q-3d2b0daa433051cc3b1b30ce99d333b14b7f6e56c7037b1b43fef4ebcb97ee61"></a>

FACT: Q-3d2b0daa4330: [GET people/804558/stats](https://statsapi.mlb.com/api/v1/people/804558/stats?stats=season&group=hitting&season=2025&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "hitting", "season": 2025, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:51.927717Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:51.927745Z"}]`. Manifest query ID `3d2b0daa433051cc3b1b30ce99d333b14b7f6e56c7037b1b43fef4ebcb97ee61`.

<a id="q-75f9564e2a041699c9d390b845fbf22a30819444d8311165ad66115f98d58ebb"></a>

FACT: Q-75f9564e2a04: [GET people/804558/stats](https://statsapi.mlb.com/api/v1/people/804558/stats?stats=gameLog&group=hitting&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:52.001020Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:52.427826Z"}]`. Manifest query ID `75f9564e2a041699c9d390b845fbf22a30819444d8311165ad66115f98d58ebb`.

<a id="q-6f985be453fbd299a636079fceedeb5f00d44bdf4fd3851fdcf9ba0307446e3d"></a>

FACT: Q-6f985be453fb: [GET people/804558/stats](https://statsapi.mlb.com/api/v1/people/804558/stats?stats=gameLog&group=hitting&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:52.501752Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:52.927912Z"}]`. Manifest query ID `6f985be453fbd299a636079fceedeb5f00d44bdf4fd3851fdcf9ba0307446e3d`.

<a id="q-f95b274d6b6bddff94664b2f54d36231082b0a075230c623495adc5463cf4fd3"></a>

FACT: Q-f95b274d6b6b: [GET people/804558/stats](https://statsapi.mlb.com/api/v1/people/804558/stats?stats=gameLog&group=hitting&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "hitting", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:53.006549Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:53.427990Z"}]`. Manifest query ID `f95b274d6b6bddff94664b2f54d36231082b0a075230c623495adc5463cf4fd3`.

<a id="q-4d8cacbea51afee8bd7a1282074c110a9e7623a6f38d3ab1a79e4f0dae578900"></a>

FACT: Q-4d8cacbea51a: [GET stats](https://statsapi.mlb.com/api/v1/stats?stats=season&group=pitching&season=2025&sportIds=14&gameType=R&limit=1&offset=0&sortStat=gamesPlayed&order=desc); parameters=`{"gameType": "R", "group": "pitching", "limit": 1, "offset": 0, "order": "desc", "season": 2025, "sortStat": "gamesPlayed", "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:53.502890Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:53.928180Z"}]`. Manifest query ID `4d8cacbea51afee8bd7a1282074c110a9e7623a6f38d3ab1a79e4f0dae578900`.

<a id="q-0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24"></a>

FACT: Q-0037c4d05cad: [GET people/813707/stats](https://statsapi.mlb.com/api/v1/people/813707/stats?stats=gameLog&group=pitching&season=2025&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 14, "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:54.112637Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:54.428251Z"}]`. Manifest query ID `0037c4d05cad8486f52ce5080b08697407804cd80c9fa098162735fd29b89d24`.

<a id="q-4655f36bffa8017ab154fbfd02cca63105e66b39af7fb322ad0723902eb4d5f5"></a>

FACT: Q-4655f36bffa8: [GET people/813707/stats](https://statsapi.mlb.com/api/v1/people/813707/stats?stats=season&group=pitching&season=2025&sportIds=14&gameType=R); parameters=`{"gameType": "R", "group": "pitching", "season": 2025, "sportIds": 14, "stats": "season"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:54.510071Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:54.928339Z"}]`. Manifest query ID `4655f36bffa8017ab154fbfd02cca63105e66b39af7fb322ad0723902eb4d5f5`.

<a id="q-4beb815804b6dc083de25306f2f4340d487916cc30ec2ae7558293b4f3b2934a"></a>

FACT: Q-4beb815804b6: [GET people/813707/stats](https://statsapi.mlb.com/api/v1/people/813707/stats?stats=gameLog&group=pitching&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-05-31); parameters=`{"endDate": "2025-05-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:55.006108Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:55.428417Z"}]`. Manifest query ID `4beb815804b6dc083de25306f2f4340d487916cc30ec2ae7558293b4f3b2934a`.

<a id="q-393f39d43381e27debf1a293f5e1e8d54bbaf34387ac7c2c756f9d5ad8c8e119"></a>

FACT: Q-393f39d43381: [GET people/813707/stats](https://statsapi.mlb.com/api/v1/people/813707/stats?stats=gameLog&group=pitching&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-07-15); parameters=`{"endDate": "2025-07-15", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:55.506622Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:55.928496Z"}]`. Manifest query ID `393f39d43381e27debf1a293f5e1e8d54bbaf34387ac7c2c756f9d5ad8c8e119`.

<a id="q-1e205506291e05d38feb6f717aa012651adcf7ebc1e2b85f130da9f6b432c9b7"></a>

FACT: Q-1e205506291e: [GET people/813707/stats](https://statsapi.mlb.com/api/v1/people/813707/stats?stats=gameLog&group=pitching&season=2025&sportIds=14&gameType=R&startDate=2025-01-01&endDate=2025-08-31); parameters=`{"endDate": "2025-08-31", "gameType": "R", "group": "pitching", "season": 2025, "sportIds": 14, "startDate": "2025-01-01", "stats": "gameLog"}`; status=ok; source=network; requested UTC=2026-10-08T19:38:56.008715Z; attempts=`[{"http_status": 200, "started_utc": "2026-10-08T19:38:56.428569Z"}]`. Manifest query ID `1e205506291e05d38feb6f717aa012651adcf7ebc1e2b85f130da9f6b432c9b7`.
