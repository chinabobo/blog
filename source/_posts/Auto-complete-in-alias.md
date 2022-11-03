---
title: Auto-complete in alias
date: 2022-11-03 12:26:10
tags:
---

/root下放了2个文件。
.complete_alias和.alias
在.bashrc里面加入source /root/.complete_alias 和source /root/.alias
可以用alias命令下的tab补全。
<!--more-->

```bash
UPDATE admins_exam_papers SET created_at=updated_at

alter table datawork_records rename to dataworkrecords

ALTER TABLE public.datawork_records RENAME TO dataworkrecords;

ALTER TABLE public.dataworkrecords RENAME COLUMN datawork_record_redis_stream_key TO redis_stream_key;

ALTER TABLE public.dataworkrecords RENAME COLUMN datawork_record_type TO record_type;

ALTER TABLE public.dataworkrecords RENAME COLUMN datawork_record_priority TO priority;

ALTER TABLE public.dataworkrecords RENAME COLUMN datawork_record_priority_score TO priority_score;

ALTER TABLE public.dataworkrecords RENAME COLUMN datawork_record_state TO status;

ALTER TABLE public.dataworkrecords RENAME CONSTRAINT datawork_records_pkey TO dataworkrecords_pkey;

ALTER INDEX public.idx_datawork_records_deleted_at RENAME TO idx_dataworkrecords_deleted_at;
```
