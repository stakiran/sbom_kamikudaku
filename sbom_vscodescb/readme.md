GitHub の SBOM Export は、SBOM Tool を使っているのではないかと思ったけど、そうでもなさそう？

結論:

- わからん
- SBOM Tool から fork してんのかな？

SBOM 生成元: https://github.com/stakiran/vscode-scb/

SBOM 生成方法 with GitHub:

- Insights > Dependency Graph > Export SBOM

# creationInfo
sbomtool:

```json
  "creationInfo": {
    "created": "2024-05-24T21:14:05Z",
    "creators": [
      "Organization: test_namespace",
      "Tool: Microsoft.SBOMTool-2.2.5"
    ]
  },
```

github

```json
  "creationInfo": {
    "created": "2024-05-24T21:09:52Z",
    "creators": [
      "Tool: GitHub.com-Dependency-Graph"
    ]
  },
```

# version

```json
  "spdxVersion": "SPDX-2.2",
```

```json
  "spdxVersion": "SPDX-2.3",
```

# package
sbomtool

```json
    {
      "name": "micromatch",
      "SPDXID": "SPDXRef-Package-020D4327FDDDFCC1BA64F919EB9349323A908399DC20B1E29BCAEE4DF01407A6",
      "downloadLocation": "NOASSERTION",
      "filesAnalyzed": false,
      "licenseConcluded": "NOASSERTION",
      "licenseDeclared": "NOASSERTION",
      "copyrightText": "NOASSERTION",
      "versionInfo": "4.0.5",
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceType": "purl",
          "referenceLocator": "pkg:npm/micromatch@4.0.5"
        }
      ],
      "supplier": "NOASSERTION"
    },
```

github

- GitHub の方が若干コンパクト、かつ洗練されてそう
    - ライセンスが licenseConcluded のみで、MIT が埋まってる
    - SPDXID は narsion から取ってきている
    - copyrightText は要らないって判断したのかな、消えてる

```json
    {
      "SPDXID": "SPDXRef-npm-micromatch-4.0.5",
      "name": "npm:micromatch",
      "versionInfo": "4.0.5",
      "downloadLocation": "NOASSERTION",
      "filesAnalyzed": false,
      "licenseConcluded": "MIT",
      "supplier": "NOASSERTION",
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceLocator": "pkg:npm/micromatch@4.0.5",
          "referenceType": "purl"
        }
      ]
    },
```

# relationships
- 情報量がほとんどない
    - github の方が間違ってるイメージ、depends_on は A → B への依存で、Aは「ルート」になるはず
    - あるいはルートをそもそもパッケージとして含めないようにしたのか

sbomtool

```json
    {
      "relationshipType": "DEPENDS_ON",
      "relatedSpdxElement": "SPDXRef-Package-020D4327FDDDFCC1BA64F919EB9349323A908399DC20B1E29BCAEE4DF01407A6",
      "spdxElementId": "SPDXRef-RootPackage"
    },
```

github

```json
    {
      "relationshipType": "DEPENDS_ON",
      "spdxElementId": "SPDXRef-com.github.stakiran-vscode-scb",
      "relatedSpdxElement": "SPDXRef-npm-micromatch-4.0.5"
    },

```

以下は sbomtool、ルートそのものも含んでいる

```json
    {
      "name": "vscode-scb-commands",
      "SPDXID": "SPDXRef-Package-0715D89EA3AE739F2C9EC0FE8148E0F2A8F71D3D491156C9D5E584F30B0F3434",
      "downloadLocation": "NOASSERTION",
      "filesAnalyzed": false,
      "licenseConcluded": "NOASSERTION",
      "licenseDeclared": "NOASSERTION",
      "copyrightText": "NOASSERTION",
      "versionInfo": "0.2.0",
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceType": "purl",
          "referenceLocator": "pkg:npm/vscode-scb-commands@0.2.0"
        }
      ],
      "supplier": "NOASSERTION"
    },

    {
      "name": "vscode-scb-syntax",
      "SPDXID": "SPDXRef-Package-983BADB70CE8B1A8BCB63C77C2AA193E31C067AA42BA0B7A6A1A473B46272F56",
      "downloadLocation": "NOASSERTION",
      "filesAnalyzed": false,
      "licenseConcluded": "NOASSERTION",
      "licenseDeclared": "NOASSERTION",
      "copyrightText": "NOASSERTION",
      "versionInfo": "0.0.4",
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceType": "purl",
          "referenceLocator": "pkg:npm/vscode-scb-syntax@0.0.4"
        }
      ],
      "supplier": "NOASSERTION"
    },

```

