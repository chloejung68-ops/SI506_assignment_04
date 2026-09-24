# 1.0 Brilliant Corners Data Model

The selected album is **Brilliant Corners**. It appears in `data/provided/release_groups.json`, and its `artist-credit.artist.id` matches the MBID provided for this task. I followed the credited MBID rather than matching the artist by name and confirmed the artist as **Thelonious Monk** in `data/provided/artists.json`.
I chose `Brilliant Corners` because it is not one of the fixed exclusions and its multiple releases show different dates.

## 1.1 Overall record shape

```text
artist: Thelonious Monk
├── id: "8e8c7417-c905-46b1-b42a-5260b4274ed4"                 [identity]
├── name: "Thelonious Monk"                                    [state]
├── sort-name: "Monk, Thelonious"                              [state]
├── type: "Person"                                              [classification]
├── gender: "Male"                                              [classification]
├── country: "US"                                                [classification]
├── life-span:                                                     [state: nested container]
│   ├── begin: "1917-10-10"                                     [state]
│   ├── ended: true                                               [state]
│   └── end: "1982-02-17"                                       [state]
├── disambiguation: ""                                           [other]
├── aliases:                                                      [state: collection]
│   └── alias:                                                     [state: nested object]
│       ├── name                                                    [state]
│       └── type                                                    [classification]
└── genres:                                                        [classification: collection]
    └── genre:                                                     [classification: nested object]
        ├── name                                                    [classification]
        └── count                                                   [other]

release-group: Brilliant Corners
├── id: "764d0d9e-68b1-39fe-b1d1-eda104926777"                  [identity]
├── title: "Brilliant Corners"                                  [other]
├── primary-type: "Album"                                        [classification]
├── first-release-date: "1957"                                  [state]
├── release-count: 25                                             [other]
├── artist-credit:                                          [other: relationship container]
│   └── artist:                                                    [other: nested object]
│       ├── id: "8e8c7417-c905-46b1-b42a-5260b4274ed4"           [other: credited MBID]
│       └── name: "Thelonious Monk"                              [other]
├── genres:                                                     [classification: collection]
│   └── genre:                                               [classification: nested object]
│       ├── name                                                    [classification]
│       └── count                                                   [other]
└── releases:                                                      [state: collection]
    └── release:                                                   [state: nested object]
        ├── id                                                      [identity]
        ├── title                                                   [other]
        ├── date                                                    [state]
        ├── status                                                  [state]
        └── disambiguation                                          [state, optional]
```

`disambiguation` is an optional field that appears on some release objects. None of the eight releases currently included for `Brilliant Corners` has this field, but it exists on release records elsewhere in the shipped data, so it is included in the release structure. When the field is absent, it should be treated as absent; no value is added.

## 1.2 Verified values for this selection

### Artist

- `id`: `8e8c7417-c905-46b1-b42a-5260b4274ed4`
- `name`: `Thelonious Monk`
- `type`: `Person`
- `gender`: `Male`
- `country`: `US`
- `life-span.begin`: `1917-10-10`
- `life-span.ended`: `true`
- `life-span.end`: `1982-02-17`
- `genres`: `jazz` (count `9`), `bebop` (count `5`), `big band` (count `3`),
  `hard bop` (count `3`), `post-bop` (count `3`), `stride` (count `2`)

### Release-group

- `id`: `764d0d9e-68b1-39fe-b1d1-eda104926777`
- `title`: `Brilliant Corners`
- `primary-type`: `Album`
- `first-release-date`: `1957`
- `release-count`: `25`
- `genres`: `jazz` (count `5`), `hard bop` (count `3`)
- Number of releases shown in the file: `8`

### Artist-to-release-group relationship

The value of `release-group.artist-credit[0].artist.id` is `8e8c7417-c905-46b1-b42a-5260b4274ed4`. This value is equal to Thelonious Monk's `id` in `artists.json`, so the two records are connected by the credited MBID rather than by comparing names. In contrast, `release-group.releases` is not the relationship to the artist; it is a collection containing the releases that belong to one release-group. Each release has its own `id`, so the identity of the release-group can be distinguished from the identity of each release.

### Actual release-object shape

The first release included for this album has the following structure:

```text
release
├── id: "4efb0872-1bb1-375d-bae7-7a39f5f52018"                  [identity]
├── title: "Brilliant Corners"                                  [other]
├── date: "1957"                                                 [state]
└── status: "Official"                                           [state]
```

The other releases use the same fields. For example, the 1961 release has the `id`
`55741e28-ebfd-45bc-8a23-5f24c2f34151` and the `date` `1961`. Therefore, `releases` is a
collection of nested release objects inside the release-group, and each release's `id` is a
separate identity from the release-group's `id`.

## 1.3 Classification decisions

I labeled `id` as identity because the MBID specifies the object.
I labeled `life-span`,`aliases`, and `releases` as state because these values can change over time.
I labeled `type`, `gender`, `country`, `primary-type`, and `genres`, along with each genre's `name`, as classification because they represent categories.
I labeled `name`, `title`, the counting metadata `release-count`, and the artist-to-album `artist-credit` relationship as other because they provide description, relationship, or metadata rather than uniquely identifying the object.
The `title` is a readable description; in this model, the MBID in `id` is what uniquely specifies the object.
