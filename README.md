# sqlx
SQL Extension


Usage

```
# pip install sqlx


import sqlx

content = """
-- ! define 关键词定义全局变量
define pronum pro002


-- ! block 定义一个可重用的内容块（类似函数），并可定义形式参数
block mash_apply_mapping(group_key)
    (
        SELECT
            mash_contract_no,
            max(unique_no) AS unique_no,
            max(apply_no) AS apply_no
        FROM
            loan_front_manager.mf_business_apply
        GROUP BY
            {group_key}
    ) AS mash_apply_mapping
endblock


-- ! for 循环语法
{% for n|m in 1|a,2|b,3|c %}

SELECT
    id,
    '{pronum}' AS product_no,
    mash_contract_no,
    -- ! if 判断语法
    {% if m == 'a' %}
        1 as is_a,
    {% endif %}
    {% if n > 1 %}
        1 as is_more,
    {% else %}
        0 as is_more,
    {% endif %}
    {n}_{m} as mark
FROM
    account_check
LEFT JOIN {mash_apply_mapping(mash_contract_no)} ON mash_apply_mapping.mash_contract_no = account_check.contract_no
WHERE
    unique_no IS NOT NULL
UNION ALL
    SELECT
        *, NULL AS mash_contract_no
    FROM
        mf_business_loan_apply_log

{% endfor %}

"""


sql = sqlx.build(content)


print(sql)


```

