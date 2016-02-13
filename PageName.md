# Compile error in  /usr/include/linux/netfilter\_ipv4/ip\_tables.h #

In file included from gipt/include/libiptc/libiptc.h:7,
> from gipt/match.h:9,
> from gipt/rule.h:9,
> from gipt/netfilter.h:8,
> from mainstream.cpp:11:
/usr/include/linux/netfilter\_ipv4/ip\_tables.h: In function ‘xt\_entry\_target**ipt\_get\_target(ipt\_entry**)’:
/usr/include/linux/netfilter\_ipv4/ip\_tables.h:222: ошибка: в арифметическом выражении использован указатель ‘VOID **’
/usr/include/linux/netfilter\_ipv4/ip\_tables.h:222: ошибка: некорректное преобразование из ‘void**’ в ‘xt\_entry\_target**’**

http://osdir.com/ml/security.firewalls.netfilter.devel/2002-09/msg00082.html — problem discussion


change /usr/include/linux/netfilter\_ipv4/ip\_tables.h
```
//at the begin
#ifdef __cplusplus
extern "C" {
#endif


static __inline__ struct ipt_entry_target *
ipt_get_target(struct ipt_entry *e)
{
    //old
    //return (void *)e + e->target_offset;
    //new
    return ((struct ipt_entry_target *)((char *)e + e->target_offset));
}


//at the end
#ifdef __cplusplus
};
#endif
```