# User-CF based

def UserSimilarity(train):
    # 建立倒排表
    item_users = {}
    for u,items in train.items():
        for i in items:
            if i not in item_users:
                item_users[i] = set()
            item_users[i].add(u)
    #print(item_users)
    
    # 建立用户对共同有行为的物品数表
    C = {} # 用户对
    N = {} # 每个用户有过行为的物品数表
    for item,users in item_users.items():
        for u in users:
            # 填N
            N[u] = N.get(u, 0) + 1
            # 填C
            for v in users:
                if u == v:
                    continue
                if u not in C:
                    C[u] = {}
                C[u].setdefault(v,0)
                C[u][v] += 1

    #print(C)
    #print(N)
    
    # 建立相似度矩阵W
    W = C.copy()
    for u, related_users in C.items():
        for v, cuv in related_users.items():
            W[u][v] = cuv / math.sqrt(N[u] * N[v])
    return W

def Recommend(user, train, W, K):
    rank = {}
    interacted_items = train[user]
    for v,wur in sorted(W[user].items(), key=lambda x: x[1], reverse=True)[0:K]:
        for i in train[v]:
            if i not in interacted_items:
                rank.setdefault(i,0)
                rank[i] += wur
    return rank

# User-IIF
# 计算用户的兴趣相似度不同
def UserSimilarity_popular(train):
    # 建立倒排表
    item_users = {}
    for u,items in train.items():
        for i in items:
            if i not in item_users:
                item_users[i] = set()
            item_users[i].add(u)
    #print(item_users)

    # 建立用户对共同有行为的物品数表
    C = {} # 用户对
    N = {} # 每个用户有过行为的物品数表
    for item,users in item_users.items():
        for u in users:
            # 填N
            N[u] = N.get(u, 0) + 1
            # 填C
            for v in users:
                if u == v:
                    continue
                if u not in C:
                    C[u] = {}
                C[u].setdefault(v,0)
                C[u][v] += 1 / math.log(1 + len(users)) ## !!!!只改了这里

    #print(C)
    #print(N)

    # 建立相似度矩阵W
    W = C.copy()
    for u, related_users in C.items():
        for v, cuv in related_users.items():
            W[u][v] = cuv / math.sqrt(N[u] * N[v])
    return W

def Recommend(user, train, W, K):
    rank = {}
    interacted_items = train[user]
    for v,wur in sorted(W[user].items(), key=lambda x: x[1], reverse=True)[0:K]:
        for i in train[v]:
            if i not in interacted_items:
                rank.setdefault(i,0)
                rank[i] += wur
    return rank

# Item-CF
def ItemSimilarity(train):
    user_items = train
    
    # 创建C和N
    C = {} 
    N = {} 
    for user,items in user_items.items():
        for i in items:
            # 填N
            N[i] = N.get(i, 0) + 1
            # 填C
            for j in items:
                if i == j:
                    continue
                if i not in C:
                    C[i] = {}
                C[i].setdefault(j,0)
                C[i][j] += 1

    #print(C)
    #print(N)
    
    # 建立相似度矩阵W
    W = C.copy()
    for i, related_items in C.items():
        for j, cij in related_items.items():
            W[i][j] = cij / math.sqrt(N[i] * N[j])
    return W

def Recommend(user, train, W, K):
    rank = {}
    action_items = train[user]
    for item,score in action_items.items():
            for j,wj in sorted(W[item].items(), key=lambda x: x[1], reverse=True)[0:K]:
                if j in action_items:
                    continue
                rank.setdefault(j,0)  # 这里要设置成setdefault,用get会报错
                rank[j] += wj * score
    return rank

# 增加解释的item-cf
def Recommend(user, train, W, K):
    rank = {}
    action_items = train[user]
    for item,score in action_items.items():
            for j,wj in sorted(W[item].items(), key=lambda x: x[1], reverse=True)[0:K]:
                if j in action_items:
                    continue
                rank.setdefault(j,{})  # 这里要设置成setdefault,用get会报错
                rank[j]['weight'] = 0
                rank[j]['weight'] += wj * score
                rank[j]['reason_'+ str(item)] = wj * score
    return rank

# IUF
def ItemSimilarity_IUF(train):
    user_items = train
    
    # 创建C和N
    C = {} 
    N = {} 
    for user,items in user_items.items():
        for i in items:
            # 填N
            N[i] = N.get(i, 0) + 1
            # 填C
            for j in items:
                if i == j:
                    continue
                if i not in C:
                    C[i] = {}
                C[i].setdefault(j,0)
                C[i][j] += 1 / math.log(1 + len(items) * 1.0)

    #print(C)
    #print(N)
    
    # 建立相似度矩阵W
    W = C.copy()
    for i, related_items in C.items():
        for j, cij in related_items.items():
            W[i][j] = cij / math.sqrt(N[i] * N[j])
    return W

def Recommend(user, train, W, K):
    rank = {}
    action_items = train[user]
    for item,score in action_items.items():
            for j,wj in sorted(W[item].items(), key=lambda x: x[1], reverse=True)[0:K]:
                if j in action_items:
                    continue
                rank.setdefault(j,0)  # 这里要设置成setdefault,用get会报错
                rank[j] += wj * score
    return rank
