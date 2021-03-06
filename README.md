# Sopia Docs

해당 레포지토리는 소피아 사용법 및 API 설명을 작성합니다.

## 기여 방법

레포지토리를 fork합니다.
작성할 문서의 언어폴더를 생성합니다.

폴더 이름은 [ISO 639](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 규격을 따릅니다.

```
예) Korean -> ko, English -> en
```

문서를 다 작성했으면 Pull Request를 생성합니다.

<br>

### 기여시 제약 사항

- 문서는 오로지 마크다운 파일로 만들 수 있습니다.
- 비슷한 부류의 문서들은 하위 폴더를 생성하여 관리할 수 있습니다.
  
  각 폴더에는 `README.md` 파일이 존재해야 합니다. 해당 파일엔 폴더 문서들의 대략적인 내용이 있어야 합니다. 자세한 내용은 새 파일을 만들어 링크할 수 있습니다.
- 문서의 이름은 `README.md` 를 제외하면 소문자와 숫자, `-` 만 사용할 수 있습니다.
```
test_docs.md (X)
testDocs.md  (X)
test-docs.md (O)
```
- 사진 등의 첨부파일은 프로젝트의 가장 상단의 [`/assets`](/assets) 폴더에서 관리해야 합니다.
