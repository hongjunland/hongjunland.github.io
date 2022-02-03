---
title: "[Spring] Pagination"
last_modified_at: 2022-02-03 20:23
categories:
    - spring
tags:
    - Java
    - page

toc: true
toc_sticky: true
toc_label: "Contents"
---

## 학습 목표

* Paging에 대해서 설명할 수 있다.
* RestAPI방식으로 paging을 할수 있다.

## 1. Pagination

* 데이터들을 모두 출력하려면 매우 많은 자원을 필요하기 때문에, 페이지로 나눠서 호출하는 행위

* API 예시

    ```shell
    Link: <https://api.github.com/repositories/123/posts?page=2>; rel="next", <https://api.github.com/repositories/123/posts?page=40>; rel="last"
    ```

* rel
    
    * next: 다음 페이지
    * last: 마지막 페이지
    * first: 첫 번째 페이지
    * prev: 이전 페이지

## 2. Spring boot에 적용(JPA)

* Controller

    ```java
    ...
    @GetMapping(value="/posts")
    public ResponseEntity<?> selectAllPosts(@RequestParam("page") @Min(0) Integer pageNum){
        return ResponseEntity.ok(postService.findAllPosts(pageNum));
    }
    ...
    ```

* Service

    ```java
    ...
    private static final Integer POST_PER_PAGE = 10;
    ... 
    @Transactional
    public List<Post> findAllPosts(Integer pageNum){
        Page<Post> page = postRepository.findAll(
                PageRequest.of(pageNum-1, POST_PER_PAGE, Sort.by(Sort.Direction.DESC, "id")
                ));
                // 아이디별 내림차순
        return page.stream()
                .map(Post::new)
                .collect(Collectors.toList());
    }
    ...
    ```

* Repository

    ```java
    ...
    public interface PostRepository extends JpaRepository<Post, Long> {
        Page<Post> findAll(Pageable pageable);
    }
    ...
    ```


