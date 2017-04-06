# GitLab API

## link
  * [GitLab API - GitLab Documentation](https://docs.gitlab.com/ce/api/)
  * [Issues - GitLab Documentation](https://docs.gitlab.com/ce/api/issues.html)
  * [Notes - GitLab Documentation](https://docs.gitlab.com/ce/api/notes.html)

## Sample

### curlの例

```
特定のラベルを持つissueを取得

curl -k -sSL -H \ 
"PRIVATE-TOKEN: xxxxxx" "https://xxxxx/api/v3/projects/xx/issues?state=opened&labels=xxxx"
```


## Tips

### id vs iid

> id vs iid 

> When you work with the API, you may notice two similar fields in API entities: id and iid. The main difference between them is scope.

* [GitLab API - GitLab Documentation](https://docs.gitlab.com/ce/api/#id-vs-iid)

id  : 全リポジトリの中でのユニークなissue id
iid : 特定のリポジトリ内でのユニークなissue id（URLで見かけるのはこれ）

### issueにコメントを残す

NotesのAPIを使用する

* [Notes - GitLab Documentation](https://docs.gitlab.com/ce/api/notes.html)
* [Notes - GitLab Documentation](https://docs.gitlab.com/ce/api/notes.html#create-new-issue-note)